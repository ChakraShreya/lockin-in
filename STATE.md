<!-- Auto-loaded into every session in this repo via .claude/hooks/load-state.py.
     Maintained by /sync-context — run it at the end of substantive AI work.
     This is the SNAPSHOT (always current). The append-only history is research/journal.md. -->

# START HERE — fresh-chat context for `lockin-in` (Nudge)

If you're a fresh agent (or a future me), read this top-to-bottom and you have
everything needed to continue.

_Last synced: 2026-07-12._

---

## About us (the team)

- Here are the 3-founder side-project squad building **Nudge**
  - Founder A profile:
    engine/PM lane per the venture doc's Part 4 charter.
    Day job: junior Salesforce dev
  - Founder B profile :
    Day Job: junior python developer, working on optimizing AI workflows.
  - Founder C profile:

  (this repo is personal, evenings-and-weekends for all three of us.)
- Hard constraints: git only when explicitly asked (see CLAUDE.local.md rule 1);
  claims discipline on all venture docs (rule 2). No company-visibility
  constraints — Claude Code is openly mentionable here.

## Why this repo exists

Home of **Project Nudge** — a lifestyle recalibration app where one vice log
recalculates both your health plan and your weekly budget ("the Loop", verified
whitespace). "Done" for this phase = the venture doc's v0.4 **build-first**
protocol executed: foundations + recruitment → build sprint (wks 1–3) → Week-3
closed-test checkpoint → live cohort (wks 4–6) as the decision gate, with
pre-committed continue/iterate/kill bands. (The WhatsApp concierge pilot was
dropped by founder decision 2026-07-12.)

## Current state / how it's going

_As of 2026-07-12 — rewritten each sync._

- **Product renamed Bounce → Nudge (2026-07-12 night)** — swept repo-wide (79
  occurrences, 22 files); the recovery mechanic formerly "the Bounce" is now
  **"The Rebound"**; venture doc file renamed to
  `Nudge_Strategic_Documentation_Suite.md`. GitHub repo renamed manually to
  `github.com/ChakraShreya/nudge` — **local git remote still needs updating**
  (not done automatically; ask explicitly).
- Venture doc now **v0.4** (build-first). No code yet; **nothing user-validated
  yet**. **The WhatsApp pilot is DROPPED (founder decision 2026-07-12)** — the
  live cohort (wks 4–6) is the first and only decision gate, with a Week-3
  closed-test checkpoint (n=12) and a ₹0 recruitment copy probe as pre-code
  signals. Gate redesign by ceo-strategist; moat metric split (delivery-view =
  hygiene; **unprompted budget-surface opens** = the moat test); cold arm added.
- **Direction: parallel tracks** — deepen all six features into build-ready
  specs now; build sprint is Weeks 1–3. Plan (amended for v0.4):
  `~/.claude/plans/research-on-the-current-reflective-bear.md`.
- **PRDs are decision-complete (2026-07-12):** all open questions answered —
  PRD-01 v1.1 and PRD-02 v0.3.1 have zero blocking unknowns. Headlines: 04:00
  IST day/week boundary; any-signal=scored; dual EMA stored, 7d shown; **money
  FINAL (after Shreya's AOV verification): flat ₹80 home cost + ₹200/300/380
  lunch baselines → credits ₹120/220/300** (the same-day per-tier call was
  superseded — full chain in PRD-02's Decision record). Alignment handoff:
  `docs/session-summary/2026-07-12_decisions-and-alignment.md` (**B & C start
  there**).
- **Prototype build plan approved:** Next.js + Supabase (from day one), ux-designer
  wireframes first, full loop minus push nudges; friends & family are the later
  test cohort. UX spec: `docs/venture/ux-spec-prototype.md`.
- Both onboarding-era doc findings closed in v0.3.
- **New riskiest assumption:** users want the prospective ₹ number at all — the
  moat ships unvalidated; first in-app evidence arrives Week 4+.
- AI tooling live: 6 role agents (ceo-strategist, product-manager,
  marketing-growth, ux-designer, market-researcher, claim-verifier), 3 flows
  (/venture-research, /venture-plan, /design-critique), grilling + handoff
  skills, context capture. See GUIDE.md.

## Verified facts

- Oura 600k study: HRV −15.6%, **lowest**-RHR +8.2% on drinking nights (same-night
  deltas only — no recovery-timeframe data) — confirmed against ouraring.com
  (2026-07-11, claim-verifier).
- **Rebaseline window re-sourced (2026-07-12):** MunichBREW II (Eur Heart J 2024)
  shows ~24h HRV/HR normalization under _heavy dosing_; WHOOP real-world shows a
  4–5-day tail. Now `[hypothesis]` — 48h Recovery window is a tunable placeholder.
- **Closed-loop moat swept (2026-07-12):** whitespace survives; the edge is the
  _prospective re-plan_, not dual display. Paceline / Aditya Birla = adjacent-opposite.
- **Feb 2026 cigarette excise overhaul** confirmed — smoking cost table updated.
- Play Store: $25 one-time fee; 12 testers / 14 days for personal-account
  closed testing — confirmed against support.google.com (2026-07-11).
- Day-1 worked example arithmetic in the doc (D=80.6, LS=68.9) re-derived
  clean (2026-07-11).

## Open failures / unresolved

- ~~"24–48h rebaseline" mis-sourced~~ — **CLOSED in v0.3** (re-sourced to
  MunichBREW II as `[hypothesis]`; window flagged as a tunable placeholder).
- ~~Domain 2 had no empty/error/recovery states~~ — **CLOSED for the 5 prototype
  screens (2026-07-12): `docs/venture/ux-spec-prototype.md`** (wireframes, all
  states, exact sensitive-state copy, tap-count audits). Three flags for
  Founder A inside its §10: past-day log editing needs an engine decision
  (F6), lunchbox-expiry footnote interpretation (F5), Recovery-Mode score
  footer reading (F7). Non-prototype screens (settings/consent/onboarding)
  still unspecced.
- ~~Food-delivery AOV unverifiable~~ — **CLOSED (2026-07-12, Shreya `270ddc2`):
  national blended AOV ₹453–458 FY25 [verified — Eternal + Swiggy filings,
  GOV÷orders re-derived]**. Residual `[hypothesis]`: Bangalore-specific AOV,
  solo-lunch segmentation, budget-tier alcohol rows — Phase-2 sweep still wanted.

## Key conclusions to date

- Differentiate on the **mechanic** — specifically the **prospective re-plan**
  (one log → forward recalculation of _both_ plans), not dual display and never
  tone; "no-shame" is crowded and retrospective money+calorie dashboards already
  exist [verified, 2026-07-12 sweep].
- ~~Cheapest evidence first: concierge pilot before any code~~ — **superseded
  2026-07-12 (founder decision): build-first.** The trade-off is owned in doc
  v0.4 Part 2: a ₹0 2-week falsification became a ~3-week build with a blunter
  metric; clawbacks = copy probe, Week-3 checkpoint, cold arm, Week-5 interviews.
  Every phase still has a kill switch; **all 3 founders must sign the gate bands
  before Week 1** (sunk-cost risk is now High).
- Manual logging is the hero trigger — AA/Gmail/SMS parsing all verified out
  of MVP reach.

## Open threads / next steps

Per the forward plan (`~/.claude/plans/research-on-the-current-reflective-bear.md`):

1. **Next AI session — scaffold + build** (per the approved prototype plan):
   Next.js + TS + Tailwind + Vitest scaffold, Supabase schema migration (PRD-01
   §2 + PRD-02 §2), then vertical slices gated by PRD fixtures (engine T1–T13;
   budget ₹200/day golden, overshoot-once, lunchbox-₹220). **Needs from Founder
   A at start: Supabase project URL + anon key into `.env.local`** (never commit).
   Git workflow now applies: branch → PR into main → rebase-merge (CONTRIBUTING.md).
2. **Founders B & C (human):** read
   `docs/session-summary/2026-07-12_decisions-and-alignment.md`; **sign the v0.4
   gate bands** (sunk-cost pre-commitment); B reviews the UX spec before building.
3. **Recruitment is the critical path (human):** 30–40 users, ≥12 committed to
   the Play closed test, with the ₹0 mock-reflow copy probe embedded in every
   recruitment conversation. Plus DPDP template evenings.
4. **Remaining AI specs (smaller now):** Recovery Mode product spec
   (product-manager) if the UX spec's states need trigger detail beyond PRD-01
   FR9; nudge spec + copy v0 (deferred — prototype ships without push nudges).
5. At build start: `/onboard-project --upgrade` from `~/ai-research` to add tdd;
   add the test command to CLAUDE.md once vitest is green.
   ~~Engine + budget-reflow PRDs~~ **DONE + decision-complete (2026-07-12).**

## Map (where things live)

- `docs/venture/Nudge_Strategic_Documentation_Suite.md` → the product, in full
- `GUIDE.md` → how to use the installed agents/flows
- `research/` → dated research digests + `journal.md` (session history)
- `docs/session-summary/` → handoffs between sessions/founders
- `.claude/agents/` + `.claude/skills/` → the BUSINESS bundle (committed openly)
