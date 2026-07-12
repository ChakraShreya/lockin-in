# PRD-02 — Budget Reflow + Lunchbox / Savings Loop

**Owner:** Product (Founder A lane — engine/PM)
**Builds in:** Phase 3, Week 4 (alongside the $D_t$/EMA balance engine)
**Source of truth:** `docs/venture/Bounce_Strategic_Documentation_Suite.md` v0.3, Domain 1 §6 ("Financial Recalibration") + Part 3 schema
**Version:** v0.3 · 2026-07-12 · _Food-delivery AOV **verified** and tags flipped (§5, FR6): national blended AOV ₹453–458 (FY25) confirmed from Eternal + Swiggy primary filings (re-derived GOV÷orders); vice-cost mid-tier now anchored, with Bangalore-specific + solo-lunch figures the only residual `[hypothesis]`. v0.2 resolved all open questions into the body (§9 removed): home cost flat ₹80 (FR6); two-baseline delivery split adopted (FR6); `{suggestion}` deterministic until the reflow-reaction gate passes (FR3/§4); all math fixed to Asia/Kolkata (FR2). v0.1 extracted Domain 1 §6 into observable requirements, added `savings_ledger`, resolved the ₹80 home-cost scope._

---

## 1. Overview — lead with the moat

This is the **financial half of the core loop, and the app's actual moat**. The
competitor sweep (`research/2026-07-12_closed-loop-competitor-sweep.md`) is
blunt about where the edge is and isn't:

- **Not the moat:** showing money-saved next to calories from one vice log.
  Reframe, Sunnyside, and DrinkControl already do this — as **retrospective
  dashboards** (a scoreboard of the past) `[verified — competitor sweep 2026-07-12]`.
- **The moat:** the **prospective re-plan**. One vice log **forward-recalculates**
  the rest of the week's spendable budget and shows _the path back_. No app
  (global or India) was found doing this `[verified — store + Crunchbase/Tracxn
  sweep 2026-07-12, absence-not-disproof]`.

Every requirement below serves that framing: the user logs once, and the number
they see is **forward-looking** ("₹200/day of fun money **till Monday**, here's
how to stay clear"), never a backward tally of damage done. If a requirement
here ever reduces to a retrospective counter, it has drifted off the moat and
should be flagged.

The second mechanic — the **Lunchbox / Leftover savings loop** — is the positive
mirror: a logged home-cooked double portion that becomes tomorrow's lunch credits
real rupees into a visible savings ledger. Same forward framing, opposite sign.

**Validation note (updated for doc v0.4 — the WhatsApp pilot is dropped):** the
one thing nobody knows — because nobody else ships it — is whether users actually
_want_ the forward ₹ number or find it like surveillance. That question is now
answered **in-app** at the live-cohort gate (unprompted budget-surface opens, plus
Week-5 interviews and the Week-0 recruitment copy probe), not by a manual pilot.
This PRD gets built first; the gate decides whether it survives.

---

## 2. Data model

### 2.1 `weekly_budgets` (from venture doc Part 3 — referenced, not changed)

`{ user_id, week_start, budget_inr, spent_inr, daily_allowance_inr }` — **no
rollover column by design** (see FR5).

| Field | Type | Meaning |
|---|---|---|
| `user_id` | fk | owner |
| `week_start` | date | Monday of this budget week, **Asia/Kolkata** (see FR2) |
| `budget_inr` | int | $W$ — the weekly discretionary pot, set at onboarding (FR1) |
| `spent_inr` | int | $\sum_{\text{this week}} c_i$ — sum of `cost_inr` on this week's vice logs |
| `daily_allowance_inr` | int (computed) | $a_{daily} = \max(0, W - \text{spent})/\text{days-left}$ (FR3) |

**Derived, not stored** (compute on read so there is no stale state):
`R = budget_inr − spent_inr`; `overshoot_inr = max(0, −R)` (FR5).

### 2.2 `vices_logged` (from venture doc Part 3 — referenced)

Already gains `cost_inr` (int, tier default, **user-editable** — FR4) and
`severity` (positive int). The budget reflow reads `cost_inr`; the balance
engine reads `severity`. One log row feeds both arrows of the loop.

### 2.3 `savings_ledger` (NEW — this PRD adds it)

The Lunchbox loop needs a durable, user-visible credit history. Not in the v0.3
schema yet — proposed here for Founder C to add alongside `weekly_budgets`.

| Field | Type | Meaning |
|---|---|---|
| `id` | pk | |
| `user_id` | fk | owner |
| `source_dinner_log_id` | fk → `vices_logged` (nullable) | the dinner that triggered the double-portion prompt |
| `credit_date` | date | the day the lunchbox was (or should be) eaten — the credit's "as of" date |
| `delivery_baseline_inr` | int | the avoided-delivery figure used (wallet-tier default, FR6) |
| `home_cost_inr` | int | the fully-loaded home-cook cost subtracted (default 80, FR6) |
| `credit_inr` | int | `max(0, delivery_baseline_inr − home_cost_inr)` |
| `status` | enum | `pending_confirm` \| `confirmed` \| `expired` |
| `created_at` | ts | prompt time |

**Running savings total** = `SUM(credit_inr) WHERE status = 'confirmed'`. It is a
query, not a stored counter (same anti-stale-state principle as `weekly_budgets`).
The ledger is **append-only and additive** — it never decrements. It is a
motivation surface, not a debt account (mirrors the no-rollover rule on the spend side).

---

## 3. Functional requirements

Each requirement = observable behavior + one concrete example + an acceptance
criterion a test or human check can verify.

### FR1 — Onboarding sets the weekly pot $W$ by wallet tier

**Behavior:** during onboarding the user picks one of three wallet tiers; the app
writes `budget_inr` = the tier default into the current week's `weekly_budgets`
row. The value is editable later but a default must always exist.

Tier defaults (venture doc §6): **₹1,500 / ₹3,000 / ₹6,000**
`[hypothesis — venture doc placeholder, Phase 2 cost-audit pending]`.

- **Given** a new user selecting the **mid** wallet tier,
  **When** onboarding completes,
  **Then** `weekly_budgets.budget_inr = 3000` for the current `week_start`.

**Acceptance:** after onboarding, exactly one `weekly_budgets` row exists for the
current Monday with `budget_inr` equal to the selected tier's default. No user
can reach the dashboard with a null/absent budget.

### FR2 — Week lifecycle: calendar Monday–Sunday, Monday 04:00 IST reset

**Behavior:** a "week" is a **fixed calendar week, Monday 04:00 to the next
Monday 04:00 Asia/Kolkata** — not a rolling 7-day window. The 04:00 boundary
follows the engine's day-boundary decision (PRD-01 Q1, founder-decided
2026-07-12): a Sunday-night 02:00 log still belongs to the outgoing week.
`week_start` is always the Monday _date_.
On the first user activity on or after a new Monday, a fresh `weekly_budgets` row
is created with `spent_inr = 0` and `budget_inr` carried from the prior week's
budget setting (the _setting_ carries; the _spend_ does not — see FR5).
"Days left in week" for FR3 = **count of days from today through Sunday,
inclusive of today** (Friday → Fri/Sat/Sun = 3).

- **Given** the last row is `week_start = 2026-07-06` (a Monday) and the user
  opens the app on Monday **2026-07-13**,
  **When** the app loads,
  **Then** a new row `week_start = 2026-07-13, spent_inr = 0` is created and the
  dashboard shows the full pot; the prior week's overshoot (if any) does **not**
  appear.

**Acceptance:** no `weekly_budgets` row's `week_start` is ever a non-Monday.
Crossing Monday 04:00 IST zeroes `spent_inr` in the new row (a Monday 02:00 log
still counts toward the outgoing week). "Days left" on Friday computes to 3; on
Sunday to 1.

**Timezone — decided, single-zone:** all budget math (week boundaries, the
Monday 00:00 reset, and "days left") is fixed to **Asia/Kolkata**, hard-coded —
there is no per-user timezone field in MVP. The Bangalore pilot is IST-only, so
the traveling-user / timezone-crossing case is **explicitly out of scope**;
revisit only if Bounce expands beyond IST users. This assumption is recorded here
so the boundary logic is unambiguous for the builder.

### FR3 — On-log recalculation + the "path back" message

**Behavior:** **every vice log** (the exact trigger — one recalculation per log
write) recomputes, for the log's week:

$$R = W - \sum_{\text{this week}} c_i, \qquad a_{daily} = \frac{\max(0, R)}{\text{days left in week}}$$

and returns a message using the template:

> **"₹{a_daily}/day of fun money till Monday — path back: {suggestion}."**

For MVP the `{suggestion}` is a deterministic string ("cook the next N days and
you're clear" where N = days-left, or "you're clear" when `R ≥ 0` and allowance
comfortably covers the tier). Phrasing is seeded from the §8 script language +
the Week-0 recruitment copy probe (doc v0.4 — there is no manual pilot to learn
from), and retuned from Week-5 interview verbatims.

**Decided — templatize only after the gate:** the deterministic string ships as-is
for MVP. We do **not** invest in `{suggestion}` NLG or harvest pilot phrasings into
templates until the **budget-reflow-reaction gate (§6) passes** — no point tuning
the copy of a mechanic that isn't yet proven wanted. Post-gate, templatization is
handed to marketing-growth's copy lane (out of this PRD).

- **Given** `W = 3000`, `spent_inr = 800`, on a **Friday** (3 days left), the user
  logs a `Philosophical` night (`cost_inr = 1200`) and then a delivery order
  (`cost_inr = 400`),
  **When** the second log writes,
  **Then** `spent_inr = 2400`, `R = 600`, `a_daily = 600/3 = 200`, and the
  message reads **"₹200/day of fun money till Monday — path back: cook Sat & Sun
  and you're clear."**

**Acceptance:** the recalculation fires on _every_ log write (not on dashboard
open, not batched). Given the worked-example inputs, the returned
`daily_allowance_inr = 200`. The message always names _the path back_, never the
deficit alone (venture doc nudge discipline).

### FR4 — Editable cost at log time, persisted to the log row

**Behavior:** at log time the cost field is **pre-filled with the tier default**
(from the cost table, §5) and is **tap-to-edit**. The value the user confirms —
default or edited — is written to `vices_logged.cost_inr` and is what `spent_inr`
sums. Editing cost changes budget math but never `severity` (health side is
independent).

- **Given** a `Late-Night Binge` delivery pre-fills `cost_inr = 500` but the user
  actually spent ₹650,
  **When** they tap the field, enter 650, and confirm,
  **Then** the row persists `cost_inr = 650`, `spent_inr` reflects 650, and the
  reflow message uses 650 — while `severity` is unchanged.

**Acceptance:** the persisted `cost_inr` equals the confirmed (possibly edited)
value, not the default. A later read of the log row returns the edited number.

### FR5 — No-rollover overshoot: show once, reset clean Monday

**Behavior:** when `R < 0` the week is overshot. The app shows the overshoot
**once, on the log that crossed zero**, then the rest of the week shows
`₹0/day` with a muted, non-alarming status line — **no debt is carried and the
next week starts clean** (venture doc §6: "the financial _never miss twice_").
Concretely:

- The **crossing log** (first log making `R < 0`) returns the overshoot message:
  > **"You're ₹{overshoot} over this week's fun budget. No debt carried — clean
  > slate Monday. Path back: {suggestion}."**
- **Subsequent logs while `R < 0`** do **not** re-fire the alarm. They show
  `daily_allowance_inr = 0` and a muted line: _"Still over — resets Monday."_
  `overshoot_inr` may update silently but is never dramatized again.
- **Monday** (FR2): new row, `spent_inr = 0`, overshoot gone. The prior overshoot
  is never summed into the new week.

- **Given** `W = 3000`, `spent_inr = 2900`, the user logs a `Blackout` night
  (`cost_inr = 1000`),
  **When** the log writes,
  **Then** `R = −900`, `overshoot_inr = 900`, `daily_allowance_inr = 0`, and the
  crossing message fires once. A further ₹300 delivery the same week shows
  `₹0/day` + "Still over — resets Monday", **not** a second alarm. The following
  Monday the dashboard shows the full ₹3,000 with no reference to the −900.

**Acceptance:** `weekly_budgets` has no rollover/debt column. Overshoot never
propagates across a `week_start` boundary. The alarm message string is emitted at
most **once per week** (on the crossing log); later same-week logs emit the muted
variant.

### FR6 — Lunchbox / Leftover loop: dinner prompt → next-day confirm → ledger credit

**Behavior:** three steps.

1. **Prompt at dinner log** (ties to the venture doc's 20:30 Dinner & Prep nudge):
   when the user logs a home-cooked dinner, offer _"Cook a double portion for
   tomorrow's lunchbox?"_ Accepting writes a `savings_ledger` row with
   `status = 'pending_confirm'`, `credit_date = tomorrow`,
   `delivery_baseline_inr` = wallet-tier lunch baseline (below),
   `home_cost_inr` = wallet-tier home cost (below),
   `credit_inr = max(0, baseline − home_cost)`.
2. **Next-day confirm:** on `credit_date`, ask _"Ate the lunchbox? (skipped the
   ₹{baseline} delivery)"_. **Confirm** → `status = 'confirmed'`, the credit
   counts toward the running total. **No/ignore by end of day** → `status =
   'expired'`, no credit (we only bank _realized_ savings — no vanity numbers).
3. **Ledger view** (FR7) shows the running confirmed total.

**Credit math per wallet tier** — `credit = lunch_delivery_baseline − home_cost`
(**founder decisions 2026-07-12**, superseding the earlier flat-₹80 proposal —
the doc's old "₹120–300/day" claim becomes **~₹150/day**):

| Wallet tier | Lunch delivery baseline | Home cost | **Credit / day** |
|---|---|---|---|
| Budget | ₹250 | ₹100 | **₹150** |
| Mid | ₹350 | ₹200 | **₹150** |
| Premium | ₹450 | ₹300 | **₹150** |

These lunch baselines sit **below** the now-verified blended food-delivery AOV of
₹453–458 (§5) — by design, since a solo everyday lunch is smaller than a blended
(often multi-person) order, and they map to Zomato's verified sub-₹250 budget-meal
segment. So the baselines are **anchored and directionally supported by primary
data**, but the specific solo-lunch figures remain `[hypothesis — Bounce's own
segmentation of the verified AOV, not a standalone published number; Bangalore
per-order sweep would confirm]`. The credit band still lands in the doc's stated
₹120–300 range — treat as **calibrated** (against a verified anchor), not yet
confirmed per-tier.

**Two-baseline split — adopted (MVP design):** the lunchbox uses a *dedicated*
lunch-delivery baseline (₹200/300/380 — a routine weekday lunch) that is
**deliberately distinct** from the vice-log delivery `cost_inr` (which can be a
₹1,600 "Weekend Write-Off" blowout). These measure different behaviors, so they
stay two numbers for MVP. Founder A signs off on this split at build time;
unifying to one delivery baseline is a Phase-2 candidate only *if* real AOV data
(§5) shows one number covers both cases.

**₹80 home-cost scope — RESOLVED (recommendation, flagged):** adopt **₹80 as a
single flat, *fully-loaded* home-cook cost** — LPG + protein + condiments +
real-world wastage included, **not** raw ingredients. The cost audit shows raw
ingredients compute to only ₹25–40/portion `[verified — BigBasket/mandi unit
prices, cost audit 2026-07-12]`, which would make the savings *look inflated*;
₹80 fully-loaded sits sensibly between that floor and the ₹85–235 paid-tiffin
ceiling `[verified — tiffin-service pricing, cost audit 2026-07-12]`. **Flag:**
₹80 remains `[hypothesis — cost audit 2026-07-12, fully-loaded scope is Bounce's
own arithmetic, not a published figure]`; stored as a single constant
`HOME_COST_INR = 80` so a Phase-2 audit can retune one number.

- **Given** a **mid**-tier user logs a home-cooked dinner and accepts the
  double-portion prompt,
  **When** they confirm eating the lunchbox the next day,
  **Then** a `savings_ledger` row `status = 'confirmed', delivery_baseline_inr =
  350, home_cost_inr = 200, credit_inr = 150` exists and the running total
  increases by ₹150.

**Acceptance:** accepting the prompt creates a `pending_confirm` row; only a
next-day **confirm** flips it to `confirmed` and moves the total; ignoring it
`expires` with zero credit. `credit_inr` for a mid-tier user = 150.

### FR7 — Visible savings ledger

**Behavior:** a screen (or dashboard module) shows the **running confirmed savings
total** and a list of recent credits (`credit_date`, `credit_inr`, source).
Framing is visceral and forward ("₹880 saved — half a tank of petrol"), per the
venture doc; the copy itself is out of this PRD's lane (marketing-growth).

- **Given** four confirmed mid-tier lunchbox credits (₹150 each),
  **When** the user opens the ledger,
  **Then** the total shows **₹600** and four line items are listed.

**Acceptance:** the displayed total equals `SUM(credit_inr WHERE status =
'confirmed')`. `pending_confirm` and `expired` rows are excluded from the total.

---

## 4. Deliberately out (and why)

| Item | Status | Why |
|---|---|---|
| Rollover / debt carry of overshoot | **Out — permanently, by design** | Debt-carry is a guilt mechanic (venture doc §6). No column, no logic. |
| Auto-detecting cost from bank/SMS/Gmail | **Out (v2 candidate)** | Manual `cost_inr` only. AA/Gmail/SMS all verified out of MVP reach (venture doc Domain 4). |
| Per-item itemized delivery/bar bills | **Out** | One `cost_inr` per log; tap-to-edit is the only granularity. Itemization is scope creep with no gate behind it. |
| Multi-currency / non-INR | **Out** | Bangalore pilot is INR-only. |
| Automated `{suggestion}` NLG / templatization | **Out until the reflow-reaction gate passes** | Suggestion is founder-written by hand in the concierge (§8); MVP ships a deterministic string. No copy investment until the moat is validated (FR3); then it moves to marketing-growth. |
| Ledger *decrement* / spending the savings | **Out** | Ledger is additive/motivational, not a wallet. No withdraw action. |
| Savings-total → budget top-up ("earn more fun money") | **Out (tempting, fenced)** | Coupling the two ledgers turns a motivation surface into an incentive to over-cook/over-log; not validated. Raise with ceo-strategist if it recurs. |
| Wallet-tier auto-inference from spend | **Out** | Tier is user-set at onboarding (FR1); no behavioral inference in MVP. |

---

## 5. Cost-table status (verified vs hypothesis)

Per repo claims discipline, every figure the reflow depends on, traced to
`research/2026-07-12_bangalore-vice-cost-audit.md`:

| Figure | Value | Status |
|---|---|---|
| Wallet-tier pot $W$ | ₹1,500 / 3,000 / 6,000 | `[hypothesis — venture doc placeholder, Phase 2 audit pending]` |
| Smoking cost | per-stick × count; Gold Flake ≈ ₹12, Classic Milds ≈ ₹24, pack ≈ ₹120–240 | `[verified — TaxGuru Feb-2026 excise notification + live Zepto/Blinkit/Instamart listings, cost audit 2026-07-12]` |
| Alcohol **premium** row | ~₹1,200–5,000/person brackets real menus | `[verified — Toit + Byg Brewski menus via Magicpin/ExploreBangalore, cost audit 2026-07-12]` |
| Alcohol **budget/mid** rows | 300/600/1000, 600/1200/2500 | `[hypothesis — cost-of-living/nightlife blogs, no per-venue budget menu fetched, cost audit 2026-07-12]` |
| Blended food-delivery AOV | ₹453–458 (FY25) | `[verified — Eternal FY25 + Swiggy FY25 filings, re-derived GOV÷orders, cost audit 2026-07-12]` |
| Food-delivery vice costs (200/250/500 …) | tier table | `[verified anchor — mid-tier ₹400–500 on national AOV]` / `[hypothesis — per-tier spread + Bangalore-specific, see §5 note]` |
| **Lunch delivery baseline** (₹200/300/380) | drives lunchbox credit | `[hypothesis — solo everyday-lunch order; bounded by verified sub-₹250 budget-meal segment but not a standalone published figure, see §5 note]` |
| Home-cook cost `HOME_COST_INR` | ₹80, fully-loaded | `[hypothesis — fully-loaded scope is Bounce arithmetic; raw-ingredient floor ₹25–40 is verified, cost audit 2026-07-12]` |
| Lunchbox credit band | ₹120–300/day | `[hypothesis — derived from two unverified inputs above]` |

**Food-delivery AOV — RESOLVED (verified 2026-07-12):** the text-native-investor-figure
path landed. Both majors publish food-delivery AOV in readable filings, each
re-derived from disclosed GOV ÷ orders: **Zomato/Eternal ₹453 (FY25)** — GOV
₹38,646 Cr ÷ 853 Mn orders = ₹453.1 — and **Swiggy ₹458 (FY25)** — GOV ₹28,783 Cr
÷ 628.9 Mn orders = ₹457.6. Two independent primary sources converge within ₹5, so
the **national blended food-delivery AOV ≈ ₹453–458** is now `[verified — Eternal
FY25 Company Overview + Swiggy FY25 Annual Report, cost audit 2026-07-12]`. This
supersedes the earlier unconfirmed "₹425" blog figure.

**What is verified vs still calibration:** the *vice-log delivery cost* mid-tier
(₹400–500) now sits on a verified anchor. **Two residual `[hypothesis]` items
remain, and they are the ones this PRD's numbers actually ride on:** (1) the AOV is
*blended national*, not Bangalore-specific (no platform publishes city-level AOV —
Bangalore metro likely sits at or above the blend); (2) the lunchbox **lunch-delivery
baseline (₹200/300/380)** is a *solo everyday-lunch* order, which is deliberately
below the blended AOV and is bounded by Zomato's verified sub-₹250 budget-meal
segment — but the specific solo figure is Bounce's own segmentation, not a published
number. Both are resolvable only by a Bangalore per-order sweep (Phase-2 cost audit).

---

## 6. Metrics & gates

This PRD does not set new gates — it inherits the venture doc's, which the reflow
exists to move. Restated for the builder:

| Gate | Where | Threshold | Status |
|---|---|---|---|
| **Unprompted budget-surface opens** — _the moat test_: % of actives opening the budget surface ≥2×/wk **not** within 15 min of a log | Live cohort (doc v0.4 Phase 2, wks 4–6) | Strong ≥ 40%, Iterate 20–40%, Kill < 20% | `[hypothesis bands — venture doc v0.4; no category benchmark for feature-level unprompted revisit]` |
| Reflow delivery-view after a vice log (hygiene, not a gate) | Live cohort | Expected ≥ 80%; < 60% = delivery/instrumentation bug, not a product verdict | `[hypothesis — venture doc v0.4]` |
| **Budget-reflow reaction** (qualitative) | Wk-0 recruitment copy probe + Wk-5 interviews (n≥6, guide = §8 script language) | Double-down = users reference the ₹ number **unprompted**; Kill = majority describe it as "being watched" | `[verified — venture doc v0.4 Appendix signals]` |
| Warm install funnel + activation (first log ≤48h) | Week-3 closed-test checkpoint (n=12) | Proceed ≥ 9/12 funnel, ≥ 8/12 activation; golden fixtures must pass | `[hypothesis bands — venture doc v0.4 checkpoint table]` |

The **unprompted-opens** gate is the one this feature lives or dies on — delivery
views are near-automatic (the message is pushed), so only unprompted returns to
the budget surface test whether the forward ₹ number (the moat) is _wanted_.
Secondary confirmations: cost-edit rate (correcting the ₹ = caring about the ₹),
reflow-screenshot shares.

---

## 7. Acceptance criteria — re-derived worked example (the canonical test fixture)

The venture doc's §6 example, restated as an end-to-end test Founder A's unit
tests must pass (mirrors the "worked examples are the acceptance tests" rule):

**Setup:** `W = 3000`; earlier this week `spent_inr = 800`; today is **Friday**
(days-left = 3, inclusive).

| Step | Action | Expected state |
|---|---|---|
| 1 | Log `Philosophical` night, default `cost_inr = 1200` | `spent_inr = 2000`, `R = 1000`, `a_daily = 1000/3 = 333` |
| 2 | Log delivery, default `cost_inr = 400` | `spent_inr = 2400`, `R = 600`, `a_daily = 600/3 = **200**` |
| 3 | Read reflow message | **"₹200/day of fun money till Monday — path back: cook Sat & Sun and you're clear."** |

**Pass condition:** after step 2, `daily_allowance_inr == 200` exactly, matching
the venture doc §6 worked example. This is the golden fixture — if it drifts, the
engine is wrong.

**Overshoot fixture (FR5):** `W = 3000`, `spent_inr = 2900`, log `cost_inr = 1000`
→ `R = −900`, `overshoot_inr = 900`, `a_daily = 0`, crossing-alarm fires exactly
once; next Monday row shows `budget_inr = 3000, spent_inr = 0`, no ₹900 reference.

**Lunchbox fixture (FR6):** mid-tier, accept prompt → next-day confirm →
`credit_inr == 150`, running total += 150.

---

## 8. ~~Phase-1 WhatsApp concierge script~~ — OBSOLETE as a concierge (doc v0.4: pilot dropped); retained as the Week-5 interview guide + copy source

The manual concierge will never run. This section survives for two jobs: (a) its
message language seeds **nudge copy v0** and the Week-0 recruitment copy probe;
(b) its scenarios are the **Week-5 structured-interview guide** (n≥6) at the live
cohort. The design principle stands everywhere: lean every message on the
**prospective re-plan** (what's left, path back), never a backward tally.

**Founder-side running state (shared sheet):** per user — `W`, this-week
`spent`, day-of-week. Founder computes `R = W − spent` and `a_daily =
max(0,R)/days-left` on each inbound vice, exactly as FR3.

**Script — onboarding (Day 0):**
> **Founder:** "Quick one to set up your week: roughly, what's a comfortable
> _fun-spend_ budget for a week — nights out, deliveries, smokes? Ballpark is
> fine, we'll tune it. Most people land ₹1.5k / ₹3k / ₹6k."
> **User:** "3k I guess"
> **Founder:** "Perfect — ₹3,000/week of fun money. I'll track it as you log.
> Just text me the vice in plain words whenever, like 'philosophical night at
> Toit' — I'll tell you where your week stands."

**Script — a vice log (the core moment):**
> **User (Fri):** "philosophical night at toit, and ordered a biryani after"
> **Founder:** _(computes: spent 800 → +1200 +400 = 2400, R=600, 3 days left,
> 600/3=200)_
> "Logged. Score absorbed it fine — one night can't wreck a rolling average.
> On money: **₹200/day of fun money till Monday.** Path back: cook Sat & Sun and
> you're clear — no dent. Want me to send a lunchbox nudge tomorrow?"

**Script — overshoot (no-rollover, said once):**
> **User (Sat):** "ended up at another bar, blackout, ~1200"
> **Founder:** _(R now −600)_
> "Got it. You're **₹600 over for the week** — but here's the thing: **no debt
> carries. Clean ₹3,000 again Monday.** Rest of this weekend's on you; I'll just
> ping the recovery + lunchbox stuff. No guilt trip from me."
> _(Any further logs Sat/Sun: founder does NOT re-nag the overshoot — just logs
> and, if asked, says "still over, resets Monday.")_

**Script — lunchbox loop (the savings mirror):**
> **Founder (20:30, dinner time):** "Cooking tonight? Make a double portion —
> tomorrow's lunchbox skips a ~₹350 delivery."
> **User:** "yeah made extra dal chawal"
> **Founder:** _(pending credit ₹350−₹200 = ₹150)_ "Nice. If you actually eat it
> tomorrow instead of ordering, that's **₹150 banked.** I'll check in."
> **Founder (next day, lunch):** "Lunchbox or delivery today?"
> **User:** "brought it 💪"
> **Founder:** "**₹150 saved — you're at ₹600 this month, most of a tank of
> petrol.** Genuinely, that adds up."

**What founders log per exchange (for the gate):** did the user **reference the ₹
number unprompted** in a later message? (Continue signal.) Did they push back
("feels like being watched")? (Kill signal — feeds the "surveillance not care"
risk in the venture doc register.)

---

## 9. Dev handoff (vertical slices, build order)

Handoff into the dev pipeline (grilling → PRD → vertical slices, via the grilling
skill + product-manager agent). Each slice is one end-to-end behavior through the
stack (schema → compute → message/UI), **not** a layer. Founder A, Week 4,
alongside the $D_t$/EMA engine (venture doc Phase 3 table).

1. **Slice A — Pot exists & resets.** Onboarding writes `budget_inr` (FR1);
   Monday-boundary creates a fresh zero-spend row (FR2). _Proves the week
   container is correct before any math rides on it._
2. **Slice B — Log recalculates & speaks.** One vice log → `spent_inr` update →
   `R`/`a_daily` compute → "path back" message (FR3), reading editable `cost_inr`
   (FR4). _This is the moat's forward reflow — ship the §7 golden fixture
   (`a_daily == 200`) as the gating unit test._
3. **Slice C — Overshoot, once, no rollover.** Crossing-log alarm + muted
   subsequent state + clean Monday (FR5). _Proves the no-debt guarantee holds
   across a week boundary._
4. **Slice D — Lunchbox credit end-to-end.** Dinner prompt → `pending_confirm`
   row → next-day confirm/expire → `credit_inr` banked (FR6). _The savings
   mirror; mid-tier `credit == 150` fixture gates it._
5. **Slice E — Ledger surface.** Running confirmed total + recent credits (FR7).
   _Makes the banked savings visible; last because it only reads what D writes._

**Grilling focus before build:** the week-boundary/timezone edge (Slice A), the
"alarm exactly once per week" invariant (Slice C), and the two-baseline delivery
split (FR6, Slice D) are the three places this is most likely to be wrong — grill
those first.
