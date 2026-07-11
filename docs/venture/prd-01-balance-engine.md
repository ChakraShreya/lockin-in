# PRD-01 — Life Score / Balance Engine (Domain 1)

**Owner:** Founder A (engine/PM lane) · **Build window:** Week-4 sprint · **Status:** build-ready
**Source of truth:** `docs/venture/Bounce_Strategic_Documentation_Suite.md` (v0.3), Domain 1 "THE CORE
BALANCE ENGINE", Part 3 "Schema deltas", §1.2 vice taxonomy.

| Version | Date | Changelog |
|---|---|---|
| 1.0 | 2026-07-12 | First build-ready spec: 3 worked examples pinned as fixtures; 7 doc-named edge cases fully specced (freeze state-machine, silent re-entry, vice stacking cap, sign invariant, Δ add-only clamp, dual-α window, m_t/D_t clamps). |

> **Claims discipline.** This is internal engine spec — most statements are design decisions, not
> external claims. The two external anchors are carried from the doc: `ω=20` and the alcohol-night
> physiology signature (lowest RHR +8.2%, HRV −15.6%) are **[verified — Oura 600k, ouraring.com,
> confirmed 2026-07-11]**. **Every severity weight, point cap, α, and threshold is a
> [calibration placeholder]** to be retuned on pilot data — the engine must store every component
> so retuning can be backtested (Part 3).

---

## 1. Overview

### 1.1 What this is
The Balance Engine is the health half of Bounce's core loop. It is a **pure, deterministic scoring
function** plus the **persistence and state-machine** around it. Given a day's inputs (sleep,
movement, nutrition, logged vices, optional wearable strain) it produces:

1. **`D_t`** — Day Quality Score, `[0,100]`, "what today earned."
2. **`LS_t`** — Life Score, an EMA of `D_t`, `[0,100]`, "the rolling portfolio." Runs in **two
   windows simultaneously** (7-day default, 30-day long view).
3. **Recovery-Mode trigger flags** — booleans consumed by Domain 2 (UI is out of scope here; the
   engine only emits the signal).

### 1.2 Design invariants (the product philosophy, as code)
- **Bounded for free.** `D_t ∈ [0,100]` and `LS_0 = 60 ∈ [0,100]` ⇒ `LS_t ∈ [0,100]` forever by
  induction. No output clamp on LS; the `[0,100]` clamp lives only on `D_t`.
- **Forgiveness Guarantee.** Worst day (`D_t=0`) costs at most `(1−α)·LS_{t-1}` = 25% at α=0.75.
- **No streak mechanics.** Unlogged days **freeze** the score — no step, no decay. Decay is a guilt
  mechanic in disguise (§4.7).
- **Severities are positive magnitudes.** The formula's minus sign applies the penalty. Storing a
  negative severity silently flips a penalty into a bonus — a bug already hit once; §4.10 makes it
  an enforced invariant with a test.

### 1.3 Scope boundary
Pure function + state machine + persistence for **health scoring only**. Budget reflow, Recovery-Mode
UI, nudges, and the Ring canvas are separate PRDs (§6 fence). The engine is wearable-free by default
(`Δ=0`); the wearable path is specced but MVP-fenced.

---

## 2. Data model

Reference: Part 3 "Schema deltas." The doc names the tables but leaves input-storage and dual-EMA
storage as gaps. This section fills them. **Bold = gap this PRD fills; plain = already in doc.**

### 2.1 `vices_logged` (existing, Part 3)
One **row per vice per log event** — stacking is multiple rows, never a summed row (§4.3).

| Column | Type | Notes |
|---|---|---|
| `user_id`, `id` | fk / pk | |
| `logged_at` | timestamptz | drives day-assignment (§ open Q1 — day boundary) |
| `vice_domain` | enum | `alcohol \| smoking \| delivery \| sugar` |
| `vice_tier` | enum/label | culturally-worded (`Blackout`, `Sutta-Chai Break`, …) |
| `severity` | int | **positive magnitude only.** DB `CHECK (severity > 0)` — §4.10 invariant |
| `cost_inr` | int | tier default, user-editable (feeds budget PRD, not this engine) |

### 2.2 **`daily_inputs`** (gap — doc stores outputs but names no input table)
The raw self-report/wearable inputs for one user-day. Vices come from §2.1 (joined by day).

| Column | Type | Notes |
|---|---|---|
| `user_id`, `date` | pk | one row per user-day |
| `sleep_hours` (`h_t`) | numeric \| null | null ⇒ Z=0, m defaults 1.0 (§4.4, open Q2) |
| `active_minutes` (`a_t`) | numeric \| null | null ⇒ S=0 |
| `protein_g` (`P_in`) | numeric \| null | for N gate |
| `energy_in` (`E_in`) | numeric \| null | for N gate |
| `rhr`, `hrv` | numeric \| null | wearable only; null ⇒ Δ=0 (§4.5) |
| `has_any_input` | bool (derived) | true if ≥1 of the above set OR ≥1 vice row exists that day → drives freeze (§4.7) |

Profile-level (from onboarding, not per-day): `body_mass_kg` (`M`), `tdee` (`E_out`),
`rhr_base`, `hrv_base`, `wallet_tier`.

### 2.3 `calculated_scores` (existing, Part 3 — with gaps filled)
Doc columns: `{ Z, S, N, m, v_sub, delta_physio, v_adj, D, alpha, LS_prev, LS }`. Gaps:

| Column | Type | Notes |
|---|---|---|
| `Z, S, N, m, v_sub, delta_physio (Δ), v_adj (V), D` | numeric | every component persisted (retune backtest) |
| **`ls7_prev, ls7`** | numeric | 7-day EMA (α=0.75) — replaces single `LS`/`alpha` |
| **`ls30_prev, ls30`** | numeric | 30-day EMA (α≈0.9355) — the long view (§4.6) |
| **`state`** | enum | `scored \| frozen` (§4.7). `frozen` rows carry LS forward, no D |
| **`reentry_flag`** | bool | true on the first `scored` day after ≥3 `frozen` days (§4.8) |
| **`silent_days`** | int | count of consecutive frozen days immediately preceding (§4.8) |
| **`recovery_triggered`** | bool | `V_t≥25 ∨ h_t<5 ∨ wearable_recovery<50%` (emit-only) |

> **Schema decision to confirm (open Q3):** dual EMA stored as **two columns** (`ls7*`,`ls30*`) on
> one row, not two rows. The doc's single `alpha`/`LS` is superseded.

---

## 3. Component formulas (canonical reference)

$$D_t = \mathrm{clamp}_{[0,100]}\Big(35 + Z_t + S_t + 15N_t - m_t V_t (1 - 0.5N_t)\Big)$$

| Symbol | Formula | Clamp |
|---|---|---|
| `Z_t` sleep credit | `25·min(1, h_t/8)` | ≤25; `h_t=null ⇒ 0` |
| `S_t` movement credit | `min(25, 0.5·a_t)` | ≤25; `a_t=null ⇒ 0` |
| `N_t` nutrition gate | `1` iff `P_in ≥ 1.2·M` **AND** `|E_in − E_out| ≤ 0.15·E_out`, else `0` | {0,1} |
| `m_t` sleep amplifier | `clamp(1 + 0.1·(8 − h_t), 1.0, 1.8)`; `h_t=null ⇒ 1.0` | `[1.0,1.8]` |
| `V_sub,t` | `Σ severity` over that day's vice rows (additive) | ≥0 |
| `Δ_t` physio top-up | `min(25, ω·(r̂ + ĥ))`, `ω=20`; no wearable ⇒ `0` | `[0,25]` |
| `r̂_t` | `max(0, (RHR_t − RHR_base)/RHR_base)` | ≥0 (floors separately) |
| `ĥ_t` | `max(0, (HRV_base − HRV_t)/HRV_base)` | ≥0 (floors separately) |
| `V_t` | `min(100, V_sub,t + Δ_t)` | ≤100 (cap **after** Δ) |
| `LS_t` (per window) | `α·LS_{t-1} + (1−α)·D_t`; seed `LS_0 = 60` | none needed |
| `α` | 7-day: `0.75`; 30-day: `1 − 2/31 ≈ 0.9355` | — |

---

## 4. Functional requirements

Each FR = observable behavior + worked example + acceptance criterion. Test IDs map to §5.

### FR1 — Day Quality `D_t`
**Behavior:** compute `D_t` from the §3 formula, clamped `[0,100]`.
**Example (Given/When/Then):** *Given* `LS_prev=65`, sleep 7h, 45-min lift, nutrition met, one
`Philosophical` (severity 25), no wearable. *When* the day is scored. *Then* `Z=21.875`, `S=22.5`,
`N=1`, `m=1.1`, `V=25`, `D = 35+21.875+22.5+15 − 1.1·25·0.5 = 80.625 → 80.6`.
**Acceptance:** T1. `D` within ±0.1 of `80.6`.

### FR2 — Nutrition gate `N_t`
**Behavior:** binary. `N=1` only when **both** protein floor and energy-window conditions hold. `N=1`
adds +15 **and** halves the vice penalty (factor `1−0.5N`).
**Example:** *Given* `M=72kg` (floor `86.4g`), `E_out=2000`. *When* `P_in=86.4, E_in=2000`. *Then*
`N=1`. *When* `P_in=86.0` (below floor). *Then* `N=0` even if energy is perfect.
**Acceptance:** T13. Boundary `P_in = 1.2M` exactly and `|E_in−E_out| = 0.15·E_out` exactly ⇒ `N=1`;
one gram under ⇒ `N=0`.

### FR3 — Adjusted vice load `V_t` (stacking + cap)
**Behavior — order of operations, exactly:** (1) `V_sub = Σ severities` (additive, **no per-vice
clamp** — EMA smoothing is the forgiveness, not a cap here); (2) compute `Δ` (§4.5; `0` in MVP);
(3) `V = min(100, V_sub + Δ)`. **The cap applies after Δ is added, never to `V_sub` alone.**
**Example:** *Given* `Blackout`(50) + `Too Stressed, Smoked a Pack`(40). *Then* `V_sub=90`. *When*
`Δ=15`. *Then* `V = min(100, 105) = 100` (not `min(100,90)+15`).
**Acceptance:** T7. `V_sub=90, Δ=15 ⇒ V=100`. `V_sub=90, Δ=0 ⇒ V=90`.

### FR4 — Sleep amplifier `m_t` + `D_t` clamp
**Behavior:** `m = clamp(1 + 0.1·(8−h), 1.0, 1.8)`. Continuous (no step cliffs). `h≥8 ⇒ 1.0`;
`h=0 ⇒ 1.8`. `h_t=null ⇒ m=1.0` (neutral — don't amplify what wasn't measured; open Q2). `D_t`
clamped `[0,100]` — a penalty that drives the raw sum negative floors at 0.
**Example:** *Given* h=3 ⇒ `m=1.5`. *Given* h=10 ⇒ `m=1.0` (clamped, not 0.8). *Given* h=0 ⇒ `m=1.8`.
**Acceptance:** T10 (`m` clamps) + T11 (`D` floors at 0 — see Day-2 fixture where raw sum = −77.9 → 0).

### FR5 — Physiology top-up `Δ` (add-only, no-wearable default)
**Behavior:** `r̂` and `ĥ` **each floor at 0 independently** — a good RHR cannot cancel a bad HRV.
`Δ = min(25, 20·(r̂+ĥ))`. **No-wearable path is the default and primary MVP path: `rhr`/`hrv` null
⇒ `Δ=0`, pure self-report.** Δ can only add, never subtract or replace what the user logged. Δ is
never surfaced as an accusation (UI concern, Domain 2).
**Example:** *Given* RHR −5% (below base) and HRV −20%. *Then* `r̂=max(0,−0.05)=0`, `ĥ=0.20`,
`Δ=20·0.20=4.0` (only the bad signal counts). *Given* no wearable. *Then* `Δ=0`.
**Acceptance:** T9. Good-RHR/bad-HRV ⇒ Δ from HRV only; null wearable ⇒ Δ=0.

### FR6 — Life Score EMA, dual window
**Behavior:** two EMAs run **simultaneously off the same `D_t` sequence**: `ls7` (α=0.75, default
display) and `ls30` (α≈0.9355, long view). **Both seed `LS_0 = 60.`** Both freeze together on
no-input days (§4.7); both step together on scored days.
**Example:** *Given* seed 60 and a first scored day `D=80.6`. *Then* `ls7 = 0.75·60+0.25·80.6 = 65.15`,
`ls30 = 0.9355·60+0.0645·80.6 = 61.33`. Same input, two trajectories; ls7 moves faster.
**Acceptance:** T12. From seed 60 and an identical D-sequence, `ls7` and `ls30` diverge as above;
both bounded `[0,100]`.

> **Note on the worked examples:** the 3 doc fixtures (T1–T3) supply `LS_prev` directly (65, 68.9,
> 51.7) and exercise the **7-day** window only. T12 is the added test that pins the 30-day window and
> the `LS_0=60` seed, which the doc states but does not work through.

### FR7 — Missing-data freeze (the state machine)
**Behavior — state machine, per user-day, evaluated at day rollover:**

```
              day has ≥1 qualifying input?
                 /                    \
              yes                      no
               |                        |
           state=scored            state=frozen
    D_t computed; ls7/ls30 step   no D_t; ls7/ls30 = ls7/ls30_prev (carried, unchanged)
```

- **"Qualifying input" = a day counts as `scored` if `daily_inputs.has_any_input` is true OR ≥1
  `vices_logged` row exists for that day.** Logging *only* sleep, *only* a vice, *only* nutrition —
  any one — makes the day `scored`.
- **Partial day** (some components present, others null): still `scored`. Missing components take
  their floor: `Z=0` (no sleep), `S=0` (no movement), `N=0` (no nutrition proof), `V_sub=0` (no
  vices), `m=1.0` (no sleep). Baseline `B=35` still applies — credit for showing up.
- **Freeze begins** at the rollover of any day with zero qualifying inputs; that day gets a `frozen`
  `calculated_scores` row carrying both LS values forward. **Freeze ends** on the next `scored` day.
  Consecutive no-input days = consecutive `frozen` rows, LS identical across all of them.
- **No decay, ever.** A frozen day does not multiply LS by α.
**Example:** *Given* `ls7_prev=61.3` and a day with no inputs and no vices. *When* rollover.
*Then* row `state=frozen`, `ls7=61.3`, no `D`. Three such days ⇒ `ls7` still `61.3`.
**Acceptance:** T4. No-input day ⇒ LS unchanged, no EMA step, `state=frozen`. T5. Vice-only day ⇒
`state=scored`, `Z=S=0, N=0, m=1.0`, D computed from those floors.

### FR8 — Silent-day re-entry (gentle, zero-penalty)
**Behavior:** **Trigger = the first `scored` day following `silent_days ≥ 3` consecutive `frozen`
days.** On return the score does **not** snap or catch up: the returning day is scored normally —
`ls7/ls30` step **once** from the carried-forward value with the new `D_t`. No decay accrued during
silence, no penalty on return. The engine sets `reentry_flag=true` and records `silent_days` on that
row; the gentle message is Domain 2/copy (not this engine — only the flag is emitted).
**Example:** *Given* `ls7=61.3` held across 4 frozen days, then a `D=90` day. *When* scored. *Then*
`ls7 = 0.75·61.3 + 0.25·90 = 68.5`, `reentry_flag=true`, `silent_days=4`. Same arithmetic as any
normal step — silence cost nothing.
**Acceptance:** T6. 3 frozen days then a scored day ⇒ single normal EMA step, `reentry_flag=true`,
`silent_days=3`. (Open Q4: is the threshold exactly `≥3` — a 2-day gap sets no flag?)

### FR9 — Recovery-Mode trigger emission (engine responsibility only)
**Behavior:** on each `scored` day, set `recovery_triggered = (V_t ≥ 25) OR (h_t < 5) OR
(wearable_recovery < 50%)`. The engine **emits the flag only**; the ≤48h state, theme, target
trimming, and early-exit logic are Domain 2 (§6 fence). The 48h window and 25/5h thresholds are
**[calibration placeholder / hypothesis]** — 48h re-sourced to MunichBREW II, a tunable placeholder.
**Example:** *Given* one `Philosophical`(25), no wearable, sleep 7h. *Then* `V_t=25 ≥ 25 ⇒
recovery_triggered=true`.
**Acceptance:** covered incidentally by T1/T2; no dedicated numeric gate (boundary `V=25` triggers).

### FR10 — Sign-convention invariant (persisted + enforced)
**Behavior — enforced invariant:** all stored `severity` values are **positive magnitudes**
(`CHECK (severity > 0)` at the DB layer; engine asserts `V_sub ≥ 0`). The penalty is applied by the
formula's leading minus sign, never by a negative stored value. A write of a negative severity is
**rejected**, not computed — because a stored `−25` would flip the penalty into a `+` bonus.
Consequently `D_t` is **monotonically non-increasing in `V_t`**: more/worse vices can only lower the day.
**Example:** *Given* a write of `severity = −25`. *Then* the write is rejected (constraint violation),
not scored as a bonus. *Given* two otherwise-identical days with `V=0` vs `V=25`. *Then* `D(V=25) <
D(V=0)`.
**Acceptance:** T8. Negative severity rejected; `D` strictly decreasing across `V=0 < V=25 < V=50`.

---

## 5. Acceptance criteria — test table

The 3 worked examples are **exact-match fixtures** (reproduce the doc's arithmetic to ±0.1). Each
edge case gets one test. Founder A writes these as the engine's unit suite before wiring UI.

| ID | Name | Inputs | Expected (assert) | FR |
|---|---|---|---|---|
| **T1** | Day 1 — drink on a great day | `LS_prev=65`; h=7, a=45, N met (`P_in≥1.2M`, energy in-band), 1×`Philosophical`(25), no wearable | `Z=21.875, S=22.5, N=1, m=1.1, V=25, D=80.6, ls7=68.9` | FR1,2,6 |
| **T2** | Day 2 — the blowout | `LS_prev=68.9`; h=4.5, a=0, N=0, vices `Blackout`(50)+`Drunk Cigs`(18)+`Late-Night Binge`(15)=83, wearable RHR +20% / HRV −35% | `Z=14.0625, S=0, N=0, m=1.35, V_sub=83, Δ≈11.0, V≈94.0, raw=−77.9 → D=0, ls7=51.7` | FR1,3,4,5 |
| **T3** | Day 3 — the bounce | `LS_prev=51.7`; h=8, a=30, N met, zero vices, no wearable | `Z=25, S=15, N=1, m=1.0, V=0, D=90, ls7=61.3` | FR1,6 |
| **T4** | Freeze on no-input day | `ls7_prev=61.3, ls30_prev=…`; zero inputs, zero vices | `state=frozen`, `ls7=61.3` (unchanged), no `D` row value, no EMA step | FR7 |
| **T5** | Partial day (vice only) | only 1×`Buzzed`(10) logged; no sleep/movement/nutrition | `state=scored, Z=0, S=0, N=0, m=1.0, V=10, D=35 − 1.0·10·1 = 25` | FR7 |
| **T6** | Silent re-entry ≥3 | `ls7=61.3` held 3 frozen days, then `D=90` day | single step `ls7=0.75·61.3+0.25·90=68.5`, `reentry_flag=true, silent_days=3` | FR8 |
| **T7** | Stacking cap after Δ | `V_sub=90` (`Blackout`50+`Pack`40); Δ=15 | `V=min(100,105)=100`; and Δ=0 ⇒ `V=90` | FR3 |
| **T8** | Sign invariant | (a) write `severity=−25`; (b) `V=0` vs `25` vs `50`, else identical | (a) write **rejected**; (b) `D(0)>D(25)>D(50)` strictly | FR10 |
| **T9** | Δ add-only, no-wearable | (a) RHR −5%, HRV −20%; (b) no wearable | (a) `r̂=0, ĥ=0.20, Δ=4.0`; (b) `Δ=0` | FR5 |
| **T10** | `m_t` clamp | h=3 / h=10 / h=0 / h=null | `m=1.5 / 1.0 / 1.8 / 1.0` | FR4 |
| **T11** | `D_t` clamp | raw component sum = −77.9 (Day-2 shape) | `D=0` (floored, not negative) | FR4 |
| **T12** | Dual-α window + seed | seed `LS_0=60`; first `D=80.6` | `ls7=65.15, ls30≈61.33`; both `∈[0,100]` | FR6 |
| **T13** | Nutrition gate boundary | `M=72` → floor 86.4g, `E_out=2000`; (a) `P_in=86.4, |E_in−E_out|=300`; (b) `P_in=86.0` | (a) `N=1`; (b) `N=0` | FR2 |

> **Fixture note (T2):** the doc writes `Δ=11.1 → V=94.1` using `ĥ≈0.354`; a clean `−35%` gives
> `ĥ=0.35 → Δ=11.0 → V=94.0`. Because the raw sum is deeply negative either way, **`D=0` and
> `ls7=51.7` are exact and identical** under both. Pin the formula (`ĥ=0.35`); assert `D=0, ls7=51.7`
> exactly and `V` within ±0.2. Flagged as open Q5 to reconcile the doc's stored `94.1`.

---

## 6. Deliberately out (MVP fence) — and why

Scope not explicitly fenced will crawl back in. Everything below is **out of this PRD**.

| Item | Status | Why |
|---|---|---|
| **Wearables / live RHR·HRV ingestion** | **Out of MVP** | India: manual-first is the verified core [Part 3]. `Δ=0` is the **primary path**; the FR5 formula + T2/T9 wearable fixtures exist only as a **v2 regression seed**, not a shipped path. |
| Recovery-Mode state, theme, target-trim, early-exit | Out (Domain 2) | Engine emits `recovery_triggered` only; UI/behavior is a separate PRD. |
| Budget reflow, savings ledger, cost math | Out (Budget-reflow PRD) | The finance half of the loop; `cost_inr` is stored here but never read by this engine. |
| Ring canvas, two-tap log UI, dual-track toggle | Out (Domain 2 / Frontend) | This PRD is logic + schema + state machine only. |
| Nudges, copy, re-entry message wording | Out (Nudge PRD / marketing) | Engine emits `reentry_flag`; the *words* are not ours. |
| Weight/α retuning tooling & admin UI | Out (post-pilot) | We **store** every component (§2.3) so retuning is *possible*; building the tuner is later. |
| Δ surfaced in UI as accusation | Out permanently | Score absorbs Δ silently by design [doc §3]. |
| Timezone/multi-device conflict resolution beyond a single day boundary | Out (open Q1) | One day-boundary rule ships; richer reconciliation deferred. |

---

## 7. Open questions for the founders

1. **Day boundary / rollover (Q1).** Local midnight, or a "night-out" cutoff (e.g. 04:00) so a 02:00
   `Blackout` counts toward the night it belongs to, not the calendar day after? This affects
   day-assignment for both scoring and budget. **Blocks:** `daily_inputs.date` derivation.
2. **Unlogged-sleep `m_t` default (Q2).** This PRD defaults `h_t=null ⇒ m=1.0` (neutral). Alternative:
   treat missing sleep as mildly penalizing. Neutral is proposed; confirm.
3. **Dual-EMA storage (Q3).** Confirm two columns (`ls7*`,`ls30*`) on one `calculated_scores` row,
   superseding the doc's single `alpha`/`LS`. And: does the 30-day view *display* in MVP, or compute
   silently for later? (Affects Domain 2, not the engine, but the store decision is now.)
4. **Re-entry threshold (Q4).** Is it exactly `≥3` frozen days (a 2-day gap sets no flag), and does the
   flag fire only on the *first* returning day? Proposed: yes and yes.
5. **T2 stored `V` reconciliation (Q5).** Doc shows `V=94.1` (ĥ≈0.354); clean `−35%` gives `94.0`.
   Pick the canonical HRV-ratio definition so the stored intermediate is deterministic. (Final `D`/`LS`
   are unaffected — both floor to `D=0`.)
6. **`has_any_input` definition (Q6).** Confirm that logging *only* a movement check-in (no vice, no
   sleep) is a `scored` day — i.e., the freeze is strictly "zero signals of any kind," not "no vice."
   Proposed: any single signal ⇒ scored.
7. **TDEE / body-mass refresh cadence (Q7).** `E_out`/`M` are onboarding profile values feeding the N
   gate. Static for MVP, or recalculated? Proposed: static, editable in profile.

---

## 8. Dev handoff (vertical slices, build order)

Pipeline: **grilling → this PRD → vertical slices** (grilling skill + product-manager agent). Each
slice is one end-to-end behavior (input → computed → persisted → assertable), never a layer. Build in
this order — each slice is shippable and testable on its own:

1. **Slice A — Pure `D_t` for a fully-logged, no-wearable day.** Inputs → all components → clamped
   `D_t`, persisted to `calculated_scores`. Lands **T1** and the N-gate (**T13**), `m`/`D` clamps
   (**T10, T11**), sign invariant + DB `CHECK` (**T8**). *This is the spine; do it first.*
2. **Slice B — Vice stacking + `V_t` cap.** Multiple `vices_logged` rows → `V_sub` → `V=min(100,·)`.
   Lands **T5** (partial/vice-only day) and **T7** (cap).
3. **Slice C — Dual-window EMA + seed.** `ls7`/`ls30` off the same `D_t`, seed 60. Lands **T3**
   (bounce) and **T12** (dual-α + seed). Now the full 3-day worked sequence T1→T2→T3 runs green
   (needs Slice D for T2's Δ).
4. **Slice D — Δ physiology path (behind a flag, off by default).** FR5 add-only clamp. Lands **T2**
   (full blowout fixture) and **T9**. Ships disabled (`Δ=0`) per the MVP fence; exists as v2 seed.
5. **Slice E — Freeze state machine.** `has_any_input` → `scored`/`frozen`, carry-forward, no decay.
   Lands **T4**.
6. **Slice F — Silent re-entry.** `silent_days` counter + `reentry_flag` on return. Lands **T6**.
   Also wire `recovery_triggered` emission (FR9) here — cheap, reads existing outputs.

**Definition of done for the sprint:** all 13 tests in §5 green, every §2.3 component persisted, and
Slice D verifiably inert (`Δ=0`) in the shipped build. Open questions Q1/Q6 must be answered before
Slice E (they define `daily_inputs.date` and `has_any_input`).
