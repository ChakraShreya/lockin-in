<!-- Auto-loaded into every session in this repo via .claude/hooks/load-state.py.
     Maintained by /sync-context — run it at the end of substantive AI work.
     This is the SNAPSHOT (always current). The append-only history is research/journal.md. -->

# START HERE — fresh-chat context for `lockin-in` (Bounce)

If you're a fresh agent (or a future me), read this top-to-bottom and you have
everything needed to continue.

_Last synced: 2026-07-12._

---

## About us (the team)

- Here are the 3-founder side-project squad building **Bounce**
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

Home of **Project Bounce** — a lifestyle recalibration app where one vice log
recalculates both your health plan and your weekly budget ("the Loop", verified
whitespace). "Done" for this phase = the venture doc's Phase 0–4 validation
protocol executed: WhatsApp concierge pilot → localized data → build sprint →
live cohort, with pre-committed continue/iterate/kill gates at each step.

## Current state / how it's going

_As of 2026-07-12 — rewritten each sync._

- Venture doc now **v0.3** (claim-hardened). No code yet — the doc
  (`docs/venture/`) is still the only artifact; **nothing user-validated yet**;
  Phase 1 (WhatsApp concierge pilot) remains the next real gate.
- **Direction set (2026-07-12): parallel tracks** — deepen all six features into
  build-ready specs *now* while Phase 0/1 pilot prep runs alongside; pilot stays
  the evidence gate, Week-4 build starts pre-armed. Full plan:
  `~/.claude/plans/research-on-the-current-reflective-bear.md`.
- Both onboarding-era doc findings are now **closed** in v0.3 (see below).
- AI tooling live: 6 role agents (ceo-strategist, product-manager,
  marketing-growth, ux-designer, market-researcher, claim-verifier), 3 flows
  (/venture-research, /venture-plan, /design-critique), grilling + handoff
  skills, context capture. See GUIDE.md.

## Verified facts

- Oura 600k study: HRV −15.6%, **lowest**-RHR +8.2% on drinking nights (same-night
  deltas only — no recovery-timeframe data) — confirmed against ouraring.com
  (2026-07-11, claim-verifier).
- **Rebaseline window re-sourced (2026-07-12):** MunichBREW II (Eur Heart J 2024)
  shows ~24h HRV/HR normalization under *heavy dosing*; WHOOP real-world shows a
  4–5-day tail. Now `[hypothesis]` — 48h Recovery window is a tunable placeholder.
- **Closed-loop moat swept (2026-07-12):** whitespace survives; the edge is the
  *prospective re-plan*, not dual display. Paceline / Aditya Birla = adjacent-opposite.
- **Feb 2026 cigarette excise overhaul** confirmed — smoking cost table updated.
- Play Store: $25 one-time fee; 12 testers / 14 days for personal-account
  closed testing — confirmed against support.google.com (2026-07-11).
- Day-1 worked example arithmetic in the doc (D=80.6, LS=68.9) re-derived
  clean (2026-07-11).

## Open failures / unresolved

- ~~"24–48h rebaseline" mis-sourced~~ — **CLOSED in v0.3** (re-sourced to
  MunichBREW II as `[hypothesis]`; window flagged as a tunable placeholder).
- UX spec (Domain 2) has no empty/error/recovery states for any screen —
  design-critique found 6 findings ≥75, worst: Recovery Mode empty states
  (the emotionally sensitive screen). Still open — the next Track-1 UX session
  (re-run `/design-critique "Domain 2"` → ux-designer writes the state specs).
- **Cost tables only partially audited** — food-delivery AOV unverifiable from
  primary sources (investor PDFs image-only); budget-tier alcohol & lunchbox
  scope still `[hypothesis]`. Finish in Phase 2 or when the budget-reflow PRD needs it.

## Key conclusions to date

- Differentiate on the **mechanic** — specifically the **prospective re-plan**
  (one log → forward recalculation of *both* plans), not dual display and never
  tone; "no-shame" is crowded and retrospective money+calorie dashboards already
  exist [verified, 2026-07-12 sweep].
- Cheapest evidence first: concierge pilot before any code; every phase has a
  kill switch. Feature-deepening specs can run in parallel without skipping the gate.
- Manual logging is the hero trigger — AA/Gmail/SMS parsing all verified out
  of MVP reach.

## Open threads / next steps

Per the forward plan (`~/.claude/plans/research-on-the-current-reflective-bear.md`):
1. ~~Engine + budget-reflow PRDs~~ **DONE (2026-07-12)** —
   `docs/venture/prd-01-balance-engine.md`, `prd-02-budget-reflow.md`. Founder A's
   Week-4 lane. Both carry open questions needing founder sign-off (schema
   `daily_inputs` table, dual ls7/ls30 columns, HRV-ratio definition, ₹80
   home-cost = fully-loaded, food-delivery AOV still unverified).
2. **Domain 2 UX (still open)** — `/design-critique "Domain 2"` → ux-designer
   writes the missing empty/error/recovery state specs + wireframes.
3. **Recovery Mode spec** (product-manager) + Recovery screens (ux-designer) —
   the emotionally sensitive screen, worst design-critique finding.
4. **Nudge spec + funnel events + copy** (product-manager + marketing-growth).
5. Phase 0 human tasks: DPDP basics, cohort recruitment (≥12 commit to Play test),
   concierge playbook (drafted in PRD-02). Competitor store-sweep **done** (2026-07-12).
6. Week-4 build sprint: `/onboard-project --upgrade` from `~/ai-research` to add
   tdd + optional FULL-tier hooks; add the test command to CLAUDE.md.

## Map (where things live)

- `docs/venture/Bounce_Strategic_Documentation_Suite.md` → the product, in full
- `GUIDE.md` → how to use the installed agents/flows
- `research/` → dated research digests + `journal.md` (session history)
- `docs/session-summary/` → handoffs between sessions/founders
- `.claude/agents/` + `.claude/skills/` → the BUSINESS bundle (committed openly)
