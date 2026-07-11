<!-- Auto-loaded into every session in this repo via .claude/hooks/load-state.py.
     Maintained by /sync-context — run it at the end of substantive AI work.
     This is the SNAPSHOT (always current). The append-only history is research/journal.md. -->

# START HERE — fresh-chat context for `lockin-in` (Bounce)

If you're a fresh agent (or a future me), read this top-to-bottom and you have
everything needed to continue.

_Last synced: 2026-07-11._

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

_As of 2026-07-11 — rewritten each sync._

- Repo just onboarded (STANDARD tier + BUSINESS bundle from `~/ai-research`).
  No code yet — the venture doc v0.2 (`docs/venture/`) is the only artifact.
- Venture doc status: desk research done, math refactored, **nothing
  user-validated yet**; Phase 1 (WhatsApp concierge pilot) is the next gate.
- AI tooling live: 6 role agents (ceo-strategist, product-manager,
  marketing-growth, ux-designer, market-researcher, claim-verifier), 3 flows
  (/venture-research, /venture-plan, /design-critique), grilling + handoff
  skills, context capture. See GUIDE.md.

## Verified facts

- Oura 600k study: HRV −15.6%, lowest-RHR +8.2% on drinking nights —
  confirmed against ouraring.com (2026-07-11, claim-verifier).
- Play Store: $25 one-time fee; 12 testers / 14 days for personal-account
  closed testing — confirmed against support.google.com (2026-07-11).
- Day-1 worked example arithmetic in the doc (D=80.6, LS=68.9) re-derived
  clean (2026-07-11).

## Open failures / unresolved

- **"24–48h rebaseline" is NOT in the cited Oura source** — the doc presents
  it as part of the study; it isn't there. Re-source from sleep/HRV literature
  or downgrade to `[hypothesis]`. It calibrates the Recovery Mode window, so
  it's load-bearing.
- UX spec (Domain 2) has no empty/error/recovery states for any screen —
  design-critique found 6 findings ≥75, worst: Recovery Mode empty states
  (the emotionally sensitive screen). Fix list is in the 2026-07-11 critique
  (re-run `/design-critique "Domain 2"` to regenerate).

## Key conclusions to date

- Differentiate on the **mechanic** (one log → dual recalculation), never on
  tone — "no-shame" is crowded [verified, doc Research Digest].
- Cheapest evidence first: concierge pilot before any code; every phase has a
  kill switch.
- Manual logging is the hero trigger — AA/Gmail/SMS parsing all verified out
  of MVP reach.

## Open threads / next steps

1. Fix the two claim-verifier findings in the venture doc (rebaseline source,
   lowest-RHR precision) → doc v0.3.
2. Address the Domain 2 UX gaps (empty/error/recovery states) — run
   `/design-critique` then let ux-designer write the missing state specs.
3. Phase 0 tasks from the doc: DPDP basics, cohort recruitment, competitor
   store-sweep evening, funnel-event definitions.
4. When the build sprint starts (Week 4): `/onboard-project --upgrade` from
   `~/ai-research` to add tdd + (optionally) FULL-tier hooks; add the test
   command to CLAUDE.md.

## Map (where things live)

- `docs/venture/Bounce_Strategic_Documentation_Suite.md` → the product, in full
- `GUIDE.md` → how to use the installed agents/flows
- `research/` → dated research digests + `journal.md` (session history)
- `docs/session-summary/` → handoffs between sessions/founders
- `.claude/agents/` + `.claude/skills/` → the BUSINESS bundle (committed openly)
