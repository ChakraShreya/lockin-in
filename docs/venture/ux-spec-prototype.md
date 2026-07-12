# UX Spec — Bounce Prototype (Domain 2 build-ready, closing the standing gap)

**Owner:** Founder B (frontend lane) · **Builds in:** Week-1–2 sprint, alongside PRD-01/PRD-02
**Source of truth:** `docs/venture/Bounce_Strategic_Documentation_Suite.md` (v0.4) Domain 2 + Domain 1
math; `docs/venture/prd-01-balance-engine.md`; `docs/venture/prd-02-budget-reflow.md`
**Version:** v1.0 · 2026-07-12 · First build-ready UX spec — Domain 2 previously had zero screen-level
detail; this fills it for the 5 prototype screens.

> **Claims discipline.** This document makes no new market/factual claims — it inherits every
> `[verified]`/`[hypothesis]` tag from the venture doc and the two PRDs (costs, formulas, thresholds)
> unchanged. Everything here is a **design decision**, not a claim.

**House heuristics applied throughout (hard requirements, not taste):**

1. Core action (vice log) ≤2 taps and <3 seconds.
2. Supportive tone in every state, including Recovery Mode — never guilt-based.
3. Wearable-free default.
4. Casual view shows **only** Life Score + budget line; Advanced Metrics toggle unfolds the rest.

**Canon locked by founders (2026-07-12, may not yet be in PRD prose):**

- Day boundary = **04:00**, not midnight. A 2 AM log belongs to "tonight." Applies to vice-log
  timestamping, check-in date labeling, Recovery Mode day transitions, and the lunchbox confirm
  window (confirm-by = next 04:00, not next midnight).
- Any single signal that day = `scored`; total silence = `frozen` (score never decays). ≥3
  consecutive frozen days → one gentle re-entry message on the first `scored` day back, zero penalty.
- UI shows **only the 7-day Life Score**. The 30-day window computes silently (per PRD-01 §4.6) and
  is never surfaced in this prototype — not even under Advanced Metrics.
- **No push nudges in this prototype.** The 08:30/11:15/20:30 nudge cadence (Domain 3) is out of
  scope; all check-in and lunchbox prompts below are **pull** (user opens the app), never push.

---

## 0. Conventions

- ASCII wireframes show the **default** state in full. Other states are specified as deltas
  ("replace X with Y") rather than redrawn box-for-box — precise, not padded.
- `[ Button ]` = tap target. `( text )` = static copy. `‹chip›` = single-tap selectable control.
  `›` at line-end = disclosure/navigation affordance.
- Every screen inherits the **Global Navigation Shell** (§1) — described once, referenced by name
  in each screen's element inventory rather than redrawn five times.

---

## 1. Global Navigation Shell (cross-cutting — read before the 5 screens)

Present, identically positioned, on **all 5 screens** unless a screen explicitly overrides it
(Recovery Mode restyles it; nothing hides it — the core action must stay reachable everywhere).

```
├──────────────────────────────────────────────┤
│   🍺     🚬     🍔     🍬        [Dashboard]  │
│ Alcohol Smoking Delivery Sugar   [Check-in]   │
│                                   [Ledger]     │
└──────────────────────────────────────────────┘
```

- **Quick-log rail** (left cluster, 4 icons): each icon **is** a domain selector. Tapping one is
  **tap 1** of the vice-log flow, from *any* screen in the app — this is the structural decision
  that makes the ≤2-tap heuristic literally true app-wide, not just when already inside a "log"
  screen. See Screen 2.
- **Tab cluster** (right, 3 items): Dashboard / Check-in / Ledger. Recovery Mode is **not** a
  fourth tab — it's a themed state of the Dashboard tab (see Screen 4 rationale) so it can't become
  a place the user feels "sent to."
- Rail icons render in full color normally; **muted/desaturated during Recovery Mode** (§Screen 4,
  Finding F2) — function unchanged, tone matched.

---

## 2. Screen 1 — Dashboard

**User goal:** one glance — "where do I stand, health and money, right now."
**Primary action:** none required (a landing/orientation screen) — CTA weight goes to the global rail.

### Wireframe (default)

```
┌────────────────────────────────────────────┐
│  Bounce                                 ⋯   │
│                                              │
│              ╭─────────────╮                │
│             ╱               ╲               │
│            │       68        │              │
│            │   Life Score    │              │
│             ╲   (7-day)      ╱               │
│              ╰─────────────╯                │
│                                              │
│   ₹200/day of fun money till Monday      ›  │
│   Path back: cook Sat & Sun and you're      │
│   clear.                                     │
│                                              │
│   [ Advanced Metrics  ▾ ]                    │
│                                              │
├──────────────────────────────────────────────┤
│   🍺     🚬     🍔     🍬   [Dashboard][Check-in][Ledger] │
└────────────────────────────────────────────┘
```

**Advanced Metrics expanded** (tap toggle → unfolds inline, no navigation):

```
   ▾ Advanced Metrics
   ┌──────────────────────────────────┐
   │ Sleep credit      21.9 / 25       │
   │ Movement credit   22.5 / 25       │
   │ Nutrition gate    ✓ met (protein  │
   │                    shield active) │
   │ This week spent   ₹2,400 / ₹3,000 │
   │                                    │
   │ Connect a wearable (optional) ›   │  ← only affordance re: RHR/HRV;
   └──────────────────────────────────┘     never shown elsewhere, never nags
```

No RHR/HRV numbers appear anywhere unless a wearable is actually connected — **wearable-free
default** applies to Advanced Metrics too, not just casual view.

### Element inventory (top to bottom)

| Element | Type | Behavior |
|---|---|---|
| Overflow menu `⋯` | tap | Settings (profile, wallet tier, data/consent — out of this spec's 5-screen scope) |
| Life Score ring | animated display, non-interactive | Renders `ls7` only. Color bands are neutral tones (blue→teal), **never red** — a low score must not read as a stoplight failure |
| Budget line | tap-to-expand (row) | Renders PRD-02 FR3 message verbatim. Tap expands an inline card: days-left, spent/total — **this in-place tap is itself an "unprompted budget-surface open" per the KPI dashboard**, so it must be reachable without leaving Dashboard |
| Advanced Metrics toggle | tap | Folds/unfolds D_t component breakdown + week spend. Collapsed by default on every app open (state does not persist as "expanded" across sessions — casual-first every time) |
| Global Nav Shell | — | §1 |

### States

**Default** — as above, returning user with a scored history.

**Empty / first-run** (seed `LS_0 = 60`, zero logs yet):

```
              ╭─────────────╮
             ╱               ╲
            │       60        │
            │  starting point │
             ╲                ╱
              ╰─────────────╯
   ₹3,000/week — nothing logged yet.
   Your week's wide open.
```

Ring renders in a muted/neutral fill (not the earned-score gradient) to signal "not yet moved,"
distinct from a low earned score. Advanced Metrics has nothing to unfold — toggle shows disabled
with copy "Log something and this fills in."

**Frozen-days return (re-entry)** — two distinct sub-states, don't conflate them:

- *Mid-freeze, opened before re-entry fires* (1–2 silent days, or ≥3 but user hasn't logged
  anything yet this session): ring shows the carried-forward score with a small "paused" tag, no
  urgency copy: `"Paused — no entries in 3 days. Whenever you're ready."` No budget alarm (budget
  math is untouched by silence).
- *Re-entry moment itself* (fires once, on the first Dashboard view **after** a `scored` day
  following ≥3 frozen days — i.e., after `reentry_flag=true` on that day's row): banner above the
  ring, dismissable, shown once only:
  > **"Welcome back. Nothing lost — your score's exactly where you left it."**
  Ring then animates from the carried value to the new post-reentry `ls7` value (per PRD-01 FR8,
  a single normal EMA step). Banner does not reappear on subsequent opens.

**Error** (data failed to load):

```
              ╭─────────────╮
             ╱               ╲
            │        !        │
             ╲                ╱
              ╰─────────────╯
   Couldn't load your score right now.
   Nothing's wrong on your end — tap to
   retry.
              [ Retry ]
```

Budget line, if cached locally, still renders from last-known-good state rather than also erroring
(partial degradation, not full blank screen) — offline-first localStorage makes this the common
case, not the exception.

**Loading** (cold start): skeleton ring (pulsing outline, no number) + skeleton budget line for
≤500ms before falling back to Error if nothing resolves.

### Tap-count audit

Viewing Dashboard = **0 taps** (landing screen). Expanding budget detail = 1 tap. Toggling Advanced
= 1 tap. Neither is the core action; both are optional secondary reveals, consistent with heuristic

# 4 (casual view default = score + budget only)

---

## 3. Screen 2 — Two-tap vice log

**User goal:** log a slip in the moment, get back to life. **This is the core action** — every
design choice here is subordinate to heuristic #1.

### Flow (entry from anywhere via the Global Nav Shell rail)

**Tap 1 — domain** (rail icon, e.g. 🍺 Alcohol) → tier sheet slides up over whatever screen was
active:

```
┌────────────────────────────────────────────┐
│ (current screen, dimmed)                    │
├──────────────────────────────────────────────┤
│        Alcohol — how far tonight?        ✕  │
│                                                │
│  ┌───────────┐ ┌───────────────┐ ┌─────────┐│
│  │  Buzzed   │ │ Philosophical │ │Blackout /││
│  │           │ │               │ │Never Again││
│  │  ₹600  ✎  │ │  ₹1,200   ✎  │ │ ₹2,500 ✎ ││
│  └───────────┘ └───────────────┘ └─────────┘│
│                                                │
│      tap a card to log — that's it            │
└────────────────────────────────────────────┘
```

**Tap 2 — tier** (tap anywhere on a card body, e.g. "Philosophical") → commits the log
immediately, at the pre-filled ₹ cost for the user's wallet tier. Tier cards show **only the
culturally-worded label and the pre-filled cost** — no severity number, no dot, no bar (see
**Finding F5** below; the doc's taxonomy line is explicit: *"logged qualitatively — never as raw
numbers"*).

- The small `✎` on the cost pill is a **separate tap target** from the card body: tapping it opens
  an inline numeric field + a `[ Confirm ₹___ ]` button, *without* logging yet. This is the
  **optional** cost-edit path (PRD-02 FR4) — it costs an extra tap by design, but only for users
  who choose precision; the default 2-tap path is untouched.
- Sheet closes without logging if the user taps `✕` or outside the sheet — **tap 1 alone commits
  nothing** (domain selection is reversible right up until the tier tap).

**Post-log confirmation** (replaces the sheet, auto-dismisses after ~4s or on tap):

```
┌────────────────────────────────────────────┐
│  ✓ Logged.                                    │
│  Score absorbed it — one night can't wreck    │
│  a rolling average.                           │
│                                                │
│  ₹200/day of fun money till Monday.           │
│  Path back: cook Sat & Sun and you're clear.  │
│                                                │
│              [ Undo ]                         │
└────────────────────────────────────────────┘
```

### Element inventory

| Element | Type | Behavior |
|---|---|---|
| Domain rail icon | tap (global) | Opens tier sheet for that domain. Tap 1. |
| Tier card (×3) | tap (card body) | Commits log at default cost. Tap 2. |
| Cost pill `✎` | tap (sub-element of card) | Opens edit-then-confirm; optional, outside the 2-tap budget |
| Sheet close `✕` | tap | Cancels, no log written |
| Post-log toast | display + 1 tap (`Undo`) | Auto-dismiss or explicit dismiss/undo |

### States

**Default** — flow above.

**Empty** — not applicable to an input flow; the "before any tap" state is simply the rail sitting
idle (no distinct empty screen).

**Loading** — none in the happy path: the write is **optimistic and local-first** (localStorage,
per the stack decision) — the toast appears instantly on tap 2, before any network round-trip.
Background sync to Supabase happens invisibly; a sync failure never blocks or delays the toast.

**Error** — background sync has failed to reconcile for >24h: a single small, non-blocking banner
appears on Dashboard (not here — the log flow itself never shows an error, since the local write
already succeeded):
> "Some logs haven't synced yet — you're still all logged locally, nothing's lost."
No retry button needed here; sync auto-retries. This is a deliberate design choice so the <3-second
core action is never held hostage by network state.

**Recovery/edge — mis-log correction (undo), specced here since it exists nowhere else:**

- **Immediate window (≤~10s, toast visible):** tap `[ Undo ]` → log row deleted, score/budget
  silently recomputed, toast copy replaces with `"Undone — nothing logged."` then dismisses. 1 tap.
- **Later same day (04:00 boundary not yet crossed):** long-press any rail icon opens **History**
  — a chronological list of today's logs, each with `Edit` / `Delete`. Tapping `Edit` reopens the
  tier sheet pre-filled with the current tier/cost; choosing a different tier or `Delete` recomputes
  silently, same supportive non-alarming tone (no "are you sure you want to undo your logging" copy
  — deletion of a log is not treated as a lapse in honesty).
- **Prior days (past the 04:00 rollover):** **read-only.** This is a deliberate prototype-scope
  boundary — editing a past day would require cascading the EMA recompute forward through every
  subsequent day, which is out of scope for the 5-screen prototype (flagged to product/engine, see
  **Finding F7**).

**Recovery/edge — overshoot-once** (this log is the one that crosses `R<0`, per PRD-02 FR5):

```
  ✓ Logged.
  Score absorbed it — one night can't wreck
  a rolling average.

  You're ₹900 over this week's fun budget.
  No debt carried — clean slate Monday.
  Path back: ease up the next couple days
  and you're set for next week.
              [ Undo ]
```

**Subsequent logs the same overshot week** (muted variant, alarm never re-fires per week):

```
  ✓ Logged.
  Still over — resets Monday.
              [ Undo ]
```

**Recovery/edge — this log triggers Recovery Mode** (`recovery_triggered` flips true): append one
line to the standard toast, non-alarming, opt-in:

```
  Also — easing your plan for the next bit.
  [ See Recovery Mode › ]
```

No forced navigation — tapping through is optional; Recovery Mode is also visible next time
Dashboard opens regardless.

### Tap-count audit vs. heuristic #1

**Domain tap (1) + tier tap (2) = log committed. 2 taps, reachable from any of the 5 screens via
the global rail — no intermediate "open logging" tap required.** Sheet-open/close animation budget
kept to ~150–250ms each so the full interaction lands under 3 seconds including tap latency.
Cost-edit (`✎`) and Undo are explicitly **outside** this budget — optional, not required for the
core action.

---

## 4. Screen 3 — Daily check-ins

**User goal:** log sleep / movement / nutrition / dinner — **any one alone is a complete,
sufficient action** (PRD-01 FR7: any single signal ⇒ `scored`, not `frozen`). The form must not
imply all fields are required.

### Wireframe (default)

```
┌────────────────────────────────────────────┐
│  Today                                        │
│  Log what you've got — one signal's enough.  │
│                                                │
│  Sleep                                        │
│  ‹<5h› ‹5–6h› ‹6–7h› ‹7–8h› ‹8h+›              │
│  Edit exact ›                                 │
│                                                │
│  Movement                                     │
│  ‹0› ‹15m› ‹30m› ‹45m› ‹60m+›                  │
│                                                │
│  Nutrition                                    │
│  Protein (g)   [        ]                    │
│  Calories      [        ]                    │
│  Target: ≥86g protein · ~2000±300 kcal        │
│                        [ Save nutrition ]     │
│                                                │
│  Dinner                                       │
│  ‹Home-cooked›        ‹Ordered in›            │
│                                                │
├──────────────────────────────────────────────┤
│  🍺  🚬  🍔  🍬     [Dashboard][Check-in][Ledger]│
└────────────────────────────────────────────┘
```

Tapping **Home-cooked** reveals inline (no navigation):

```
  Cook a double portion for tomorrow's
  lunchbox?
        [ Yes, log it ]   [ No thanks ]
```

Tapping **Ordered in** reveals inline (dismissible, never forced):

```
  Want to log this as tonight's vice too?
        [ Log delivery › ]   [ Skip ]
```

`Log delivery ›` opens the Screen 2 tier sheet **pre-set to the Delivery domain** — saves the user
a rail tap, still lands as a normal 2-tap-from-here-on log.

### Element inventory

| Element | Type | Behavior |
|---|---|---|
| Sleep chips | tap, single-select | Instant save on tap, no separate confirm |
| "Edit exact" | tap → inline stepper | For users who want a precise number instead of a bucket |
| Movement chips | tap, single-select | Instant save on tap |
| Protein / Calories fields | numeric keyboard input | Requires explicit `[ Save nutrition ]` tap (2 fields, so one confirm is appropriate — this is the one section that isn't single-tap, and isn't the core action, so heuristic #1 doesn't bind it) |
| Nutrition gate indicator | display, neutral | Appears only once both fields are non-empty; reads "Gate: met ✓" or nothing (never "Gate: NOT met ✗" in alarming red) |
| Dinner chips | tap, single-select | Reveals the relevant inline follow-up (lunchbox prompt or vice-log suggestion) |

### States

**Empty (default, nothing logged today):** exactly the wireframe above — all sections neutral, no
red asterisks, no "3 of 4 incomplete" progress framing anywhere. Nutrition gate indicator is absent
entirely (not "0% met") until both fields have values.

**Partial (some fields saved):** each completed section collapses to a compact confirmed summary
with a `change` link, e.g. `Sleep ✓ 7h  ·  change`. Untouched sections stay in their full neutral
chip-row form — never flagged as missing, consistent with "any one signal is fine."

**Error (a field save fails):** inline, per-field, non-blocking:
> "Couldn't save that — tap to retry."
Never blocks other fields from saving; never resets what already succeeded.

**Recovery/edge:**

- Editing a value already saved today: tap the chip again (or `change` link) — always editable
  same-day.
- Crossing the 04:00 boundary: the day "closes." Attempting to edit yesterday's check-in shows:
  > "This day's closed — tomorrow's a fresh one."
  (read-only, matches Screen 2's same-day-edit scope decision)
- Lunchbox `Yes, log it` writes a `pending_confirm` row in `savings_ledger` (PRD-02 FR6) — the
  confirm/expire half of that flow lives on Screen 5, not here.

### Tap-count audit

Sleep-only log = 1 tap. Movement-only = 1 tap. Nutrition = 2 field entries + 1 save tap (not the
core action, so not heuristic-bound, but flagged as friction in Findings — below threshold on its
own, since no stated heuristic pins nutrition-entry speed). Home-cooked → lunchbox yes = 2 taps
total (dinner chip + Yes).

---

## 5. Screen 4 — Recovery Mode

**User goal:** get through the next stretch gently — this is the emotionally sensitive screen; every
line of copy and every visible element must pass the "supportive, never guilt" bar.

**Where it lives:** this is **not** a separate nav destination. It's a themed replacement of the
Dashboard tab's content while `recovery_triggered` is active — reinforces that Recovery Mode is a
mode of the same app, not a place you get sent (see **Finding F1**).

### Wireframe (default, dark theme)

```
┌────────────────────────────────────────────┐
│  (dark, low-stimulation background)          │
│                                                │
│         Taking it easy for a bit.             │
│                                                │
│              ◍  ◍  ◍  ○                      │
│         3 of 4 glasses today                  │
│         [ + Log a glass of water ]            │
│                                                │
│         A short walk, whenever you're         │
│         ready.                                │
│         [ Log a walk ]                        │
│                                                │
│                                                │
│  Life Score: 51 · ₹200/day till Monday        │  ← muted footer, see Finding F4
│                                                │
│  I'm feeling good — exit Recovery Mode  ›     │
├──────────────────────────────────────────────┤
│  🍺  🚬  🍔  🍬  (muted)  [Dashboard][Check-in][Ledger] │
└────────────────────────────────────────────┘
```

Explicitly **absent**: workout/strain rings, movement-minute targets, streaks, any comparison to
prior days, any countdown timer toward the 48h window (a visible clock reads as a sentence, not
support — the window exists on the backend only; see PRD-01 §4.9, the 48h figure is itself a
`[hypothesis]` placeholder, another reason not to surface it as a hard deadline).

### Element inventory

| Element | Type | Behavior |
|---|---|---|
| Hydration glasses (4) | tap to fill next glass | +250ml per tap, target = +1L above normal, per venture doc §7 |
| "Log a walk" | tap | Binary logged/not — no distance, pace, or calorie tracking |
| Life Score + budget footer | display, muted | Kept per heuristic #4 — see Finding F4 |
| "Exit Recovery Mode" | tap | Manual override, always visible, low visual emphasis but never hidden in a submenu |
| Global Nav Shell | — | rail icons desaturated/muted; still fully functional (logging must stay possible everywhere) |

### States

**Entry moment** (first view immediately after `recovery_triggered` flips true): a brief full-bleed
transition card, dismissible with a tap, precedes the standard layout:
> **"Taking it easy for a bit. Hydration and a short walk — that's the whole plan today."**

**Mid-recovery** (subsequent days within the window, no auto-exit yet): identical layout; glass/walk
counts reset each day; a skipped prior day is **never** shown as a miss (same freeze philosophy —
no streak, no shaming for a quiet day).

**Early-exit** (engine condition met: next day `h≥7` and zero vices — PRD-01/doc §7): a banner
appears above the hydration section, offering a choice rather than forcing exit:

```
  Looks like you're steady again.
  [ Ease off Recovery Mode ]   [ Stay in it a bit longer ]
```

**Manual exit** (user taps the override link): brief confirmation toast, then returns to the normal
Dashboard theme:
> **"You know your body best. Exiting Recovery Mode — your regular plan's back."**

**Re-trigger while already in Recovery** (another logged vice re-evaluates `recovery_triggered=true`
mid-window): no alarming "reset" language — the window silently extends:
> "Noted — sticking with the easy plan a little longer."

**Empty** (0 glasses, walk not logged yet today): the default numbers shown above (`○ ○ ○ ○`, no
walk logged) — framed as a fresh start each day, never as a deficit ("0/4" is avoided; the fill
visual communicates progress without a fraction).

**Error** (hydration/walk tap fails to save): inline, non-blocking, beneath the affected button:
> "Couldn't log that — tap to retry."

### Tap-count audit

Logging a glass = 1 tap. Logging a walk = 1 tap. Manual exit = 1 tap + toast (no confirmation
dialog interrupting — a "are you sure?" on exiting Recovery would itself undermine the "manual
override always available" spec line).

---

## 6. Screen 5 — Savings ledger + lunchbox confirm

**User goal:** see the running savings total; confirm/decline yesterday's lunchbox.

### Wireframe (default)

```
┌────────────────────────────────────────────┐
│  Savings                                      │
│                                                │
│              ₹880 saved                       │
│         ≈ about half a tank of petrol         │
│                                                │
│  ┌──────────────────────────────────────┐   │
│  │ Ate the lunchbox?                       │   │
│  │ (skipped the ~₹300 delivery)            │   │
│  │        [ Yes, banked it ]   [ No ]      │   │
│  └──────────────────────────────────────┘   │
│                                                │
│  Recent                                       │
│  ────────────────────────────                 │
│  Tue    Lunchbox              +₹220           │
│  Mon    Lunchbox              +₹220           │
│  Fri    Lunchbox              +₹220           │
│  Wed    Lunchbox              +₹220           │
│                                                │
├──────────────────────────────────────────────┤
│  🍺  🚬  🍔  🍬     [Dashboard][Check-in][Ledger]│
└────────────────────────────────────────────┘
```

Pending-confirm card renders **only** if a `savings_ledger` row exists with
`status='pending_confirm'` and `credit_date = today`. Per PRD-02 FR6, there is at most one such row
at a time (the window is next-day-only).

> *(Illustrative amounts above predate the 2026-07-12 constants decision — build against PRD-02
> v0.2: baselines ₹250/350/450, credits ≈₹150/day.)*

### Element inventory

| Element | Type | Behavior |
|---|---|---|
| Running total | display | `SUM(credit_inr) WHERE status='confirmed'` — query, not stored counter (PRD-02 §2.3) |
| Petrol-equivalence line | display | Visceral framing per PRD-02 FR7; copy ownership is marketing-growth's lane, placeholder shown |
| Pending-confirm card | conditional, 2 tap targets | `Yes, banked it` → `confirmed`, credit added, total animates up. `No` → `expired`, zero credit, card dismisses immediately (no need to wait for day-end if user answers proactively) |
| Recent list | display, read-only | Reverse-chronological, `confirmed` only — `pending`/`expired` never appear here |

### States

**Default** — as above.

**Empty** (no confirmed credits yet, first-run): total and equivalence line replaced with:
> "Nothing banked yet — cook a double portion tonight and tomorrow's lunch can start it."
No `Recent` section header shown at all (not an empty list with a header — a header with nothing
under it reads like a broken screen).

**Pending awaiting confirm** — the card shown in the default wireframe; this is the primary daily
state whenever a dinner log opted into the double-portion prompt yesterday.

**Expired (declined or ignored past 04:00):** per PRD-02 FR6, excluded from the total entirely —
"we only bank realized savings, no vanity numbers." Rather than total silence (which risks reading
as the app having forgotten to ask), a single muted, non-persistent footnote appears once:
> "No lunchbox logged yesterday."
It shows on the next ledger view only, then never resurfaces for that day — informational, not a
miss-tracker, and never phrased as "you skipped it" (see **Finding F6** — this is an interpretation
beyond PRD-02's literal silence, flagged for confirmation).

**Error** (ledger totals fail to load):
> "Couldn't load your savings — tap to retry. Nothing's lost, just not showing right now."
`[ Retry ]`

**Error, narrower** (the confirm-tap itself fails to save — distinct from natural expiry): the
pending card **stays pending**, does not silently flip to expired due to a network blip:
> "Didn't save — tap to try again."
This distinction (network failure vs. genuine end-of-day non-response) must be enforced server-side:
only the 04:00 rollover job expires a row, never a failed client write.

### Tap-count audit

Confirming the lunchbox = 1 tap. Declining = 1 tap. Viewing the ledger = 0 taps (nav only).

---

## 7. Sensitive-state copy library (consolidated, exact strings)

| State | Screen | Copy |
|---|---|---|
| Recovery Mode — entry | 4 | "Taking it easy for a bit. Hydration and a short walk — that's the whole plan today." |
| Recovery Mode — early exit offer | 4 | "Looks like you're steady again." / `[ Ease off Recovery Mode ]` `[ Stay in it a bit longer ]` |
| Recovery Mode — manual exit confirm | 4 | "You know your body best. Exiting Recovery Mode — your regular plan's back." |
| Recovery Mode — re-trigger mid-window | 4 | "Noted — sticking with the easy plan a little longer." |
| Reflow — normal path back | 2 | "Logged. Score absorbed it — one night can't wreck a rolling average. ₹{a_daily}/day of fun money till Monday. Path back: {suggestion}." |
| Overshoot — crossing log (once/week) | 2 | "You're ₹{overshoot} over this week's fun budget. No debt carried — clean slate Monday. Path back: {suggestion}." |
| Overshoot — muted subsequent logs | 2 | "Still over — resets Monday." |
| Re-entry — after ≥3 frozen days | 1 | "Welcome back. Nothing lost — your score's exactly where you left it." |
| Freeze — mid-silence, opened before re-entry | 1 | "Paused — no entries in {n} days. Whenever you're ready." |
| First-ever log | 2 | "Logged — first one! Your Life Score starts moving from here." |
| Undo confirmation | 2 | "Undone — nothing logged." |
| Lunchbox — empty ledger | 5 | "Nothing banked yet — cook a double portion tonight and tomorrow's lunch can start it." |
| Lunchbox — expired footnote | 5 | "No lunchbox logged yesterday." |
| Generic error (data load) | any | "Couldn't load {X} right now. Nothing's wrong on your end — tap to retry." |
| Generic error (save/sync) | any | "Couldn't save that — tap to retry." |

Every entry avoids: deficit-only framing, red/alarm color implication, second-person accusation
("you failed to…"), and comparison to a prior best. Every reflow/overshoot line names the **path
back**, per the venture doc's explicit nudge-copy discipline ("every message shows the path back,
never the deficit alone").

---

## 8. Transition map

```
        ┌──────── long-press any rail icon ────────┐
        ▼                                            │
   [History list] ◄── Screen 2 (tier sheet) ◄── tap domain icon (any screen)
        │                    │
     tap Edit           tap tier card
        │                    │
        ▼                    ▼
   [tier sheet, prefilled]  [post-log toast] ── tap "See Recovery Mode ›" ──► Screen 4
                                                        (only if recovery_triggered)

   Screen 1 (Dashboard) ◄──tabs──► Screen 3 (Check-in) ◄──tabs──► Screen 5 (Ledger)
        ▲                                  │
        │                          "Home-cooked" → Yes
        │                                  ▼
        └──────────────── writes pending_confirm row, surfaces on Screen 5 next 04:00

   Screen 1 (Dashboard) ──[recovery_triggered=true, next open]──► Screen 4 (Recovery Mode,
                                                                    same tab, themed)
   Screen 4 ──manual override or early-exit accept──► Screen 1 (Dashboard, normal theme)
```

---

## 9. Tap-count audit summary

| Screen | Action | Taps | Bound by heuristic #1? |
|---|---|---|---|
| 2 — Vice log | domain → tier | **2** | **Yes — this is the core action** |
| 2 — Vice log | + cost edit (optional) | +1 | No (opt-in) |
| 2 — Undo | within toast window | 1 | No |
| 1 — Dashboard | view | 0 | n/a |
| 1 — Dashboard | expand budget detail | 1 | No |
| 3 — Check-in | sleep or movement, single field | 1 | No |
| 3 — Check-in | nutrition (2 fields + save) | 3 | No |
| 3 — Check-in | dinner → lunchbox yes | 2 | No |
| 4 — Recovery | log water / log walk | 1 each | No |
| 4 — Recovery | manual exit | 1 | No |
| 5 — Ledger | confirm/decline lunchbox | 1 | No |

Only Screen 2 is bound by the ≤2-tap/<3s heuristic — and it is met **from every screen in the app**,
not just when a "logging" screen is already open, because the domain icon lives in the global rail.

---

## 10. Findings — design risks caught and resolved while specifying (confidence-scored)

Ordered by confidence × impact. All ≥75 are surfaced per the confidence rubric; **+4 below
threshold** not detailed here (nutrition-entry friction — no stated heuristic pins check-in speed;
chip-vs-slider control choice for sleep/movement; bottom-nav icon density on small viewports;
ring color-band exact hex values — all taste-level, buildable either way).

1. **[92] Tier chips must never show a severity number or visual magnitude indicator.**
   Quotes doc §1.2: *"Four domains, three tiers each, logged qualitatively — never as raw numbers."*
   An earlier draft of this spec included a "severity dot" on tier cards for at-a-glance weight —
   removed. **Fix (applied):** tier cards in Screen 2 show only the culturally-worded label and the
   ₹ cost, nothing else.

2. **[85] Recovery Mode must not become an app-wide dark takeover.**
   Applying the dark theme to Check-in/Ledger too would risk reading as confinement rather than
   care, in tension with *"supportive tone in every state ... never guilt-based."* **Fix
   (applied):** dark theme is scoped to the Dashboard tab's Recovery content only; other tabs keep
   normal theming, just with a muted rail (Finding 3).

3. **[80] Full-color vice-logging icons sitting directly under a "taking it easy" screen undercuts
   the calm framing**, even though the rail must stay functional everywhere (core action can't be
   hidden). **Fix (applied):** rail icons render desaturated/muted during Recovery Mode — same tap
   targets, quieter visual weight.

4. **[78] "Ordered in" immediately offering a vice-log suggestion risks feeling like being caught**,
   in tension with the no-guilt heuristic — an ordinary dinner choice sitting one tap from a vice
   modal. **Fix (applied):** the suggestion is opt-in with an equally-weighted `Skip`, never
   auto-opens the tier sheet. **Flag:** final copy tone here should get a pass from whoever owns
   nudge-copy discipline (Domain 3 lane) before ship — this spec fixes the *interaction*, not the
   final *wording*.

5. **[78] Fully-silent lunchbox expiry (per PRD-02's literal "no vanity numbers" text) risks
   reading as app malfunction** to a user who's had several confirmed credits and then sees nothing.
   **Fix (applied):** a single muted, non-repeating footnote ("No lunchbox logged yesterday") —
   informational, not a miss-tracker. **Flag:** this is an interpretation *beyond* PRD-02's literal
   text; confirm with Founder A (PRD-02 owner) that a footnote doesn't cross into "vanity number"
   territory PRD-02 was explicitly avoiding.

6. **[77] Mis-log correction has no engine-side spec for editing a past (already-rolled-over)
   day** — this UX spec scopes Undo/History edits to same-day-only (before the 04:00 boundary) to
   avoid an EMA-recompute cascade that isn't specced anywhere in PRD-01. Real user impact: a user
   who mis-logs and doesn't notice until the next day has **no way to correct it**, and the wrong
   entry permanently shapes their `ls7` trajectory. **Flag to product/engine (Founder A):** this
   is a functional gap this UX spec cannot close alone — it needs an engine-level decision on
   whether historical recompute is in scope for a later slice.

7. **[76] Tension between "casual view = Life Score + budget only" and Recovery Mode's "workout
   rings hidden."** The doc doesn't say to hide the Life Score itself in Recovery Mode — only
   exercise/strain rings — but a literal reading of "single walk+water focus" could argue for
   hiding everything else too. **Fix (applied):** Screen 4 keeps a muted one-line `Life Score: 51 ·
   ₹200/day till Monday` footer, satisfying heuristic #4 literally. **Flag:** confirm this reading
   with product — if Recovery Mode is meant to be a full metrics blackout, the footer should come
   out.

---

## 11. Deliberately out of this prototype spec (fence, matching venture-doc scope)

| Item | Why out |
|---|---|
| Onboarding flow (wallet-tier pick, seed budget) | Assumed complete per PRD-02 FR1 — dashboard can never be reached with a null budget; onboarding itself isn't one of the 5 named screens |
| Push nudges (08:30 / 11:15 / 20:30) | Explicit prototype-scope exclusion per this task's brief |
| Settings / consent / data-deletion screens | DPDP-required but not one of the 5 named screens |
| Swipe/double-tap/long-press as *primary* interactions | Spec'd as secondary accelerators only (History via long-press) — primary controls are always visible tap targets, since gestures alone aren't discoverable and the core action can't depend on an undiscoverable gesture |
| Historical-day log editing (cascading EMA recompute) | Out per Finding 6 — flagged to product/engine, not resolved here |
| 30-day Life Score display | Explicitly out per founder canon — computed silently, never surfaced |
