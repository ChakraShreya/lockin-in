# Handoff — 2026-07-12: All build-blocking decisions made (read this, Founders B & C)

**Audience:** all three founders. Written so you can catch up in 5 minutes without
reading the session logs. Everything below is already landed in the docs it names.

---

## TL;DR

1. **The WhatsApp concierge pilot is dropped** (Founder A's call). We build first;
   the **live cohort (weeks 4–6) is now the first and only evidence gate**. Venture
   doc is at **v0.4** with the redesigned protocol.
2. **Every open question in PRD-01 and PRD-02 is now answered** — the engine and
   budget-reflow specs are decision-complete and buildable (decisions table below).
3. **Next up:** a prototype (full loop, no push nudges) on Next.js + Supabase,
   shipped to friends & family as the test cohort. UX wireframe spec is being
   written now (`docs/venture/ux-spec-prototype.md`).
4. **Action needed from B & C:** read the decisions table + sign off on the v0.4
   gate bands (see "What we need from you").

---

## The protocol change (doc v0.4, Part 2)

- Old: Phase 0 → WhatsApp pilot (the big gate) → data week → build → live cohort.
- New: **Phase 0 (wk 0) → build sprint (wks 1–3) → Week-3 closed-test checkpoint
  (n=12, funnel + activation only) → live cohort (wks 4–6) = the decision gate.**
- What we lose: the cheap pre-code test of the moat ("does anyone want the ₹
  number?"). It now ships unvalidated — this is the **new riskiest assumption**.
- What claws the signal back (all in doc v0.4):
  - **₹0 recruitment copy probe:** while recruiting testers, show the mock reflow
    message ("₹200/day till Monday…") and note verbatim reactions.
  - **Week-3 checkpoint** with the 12 committed testers (fix-before-spend).
  - **Cold arm:** 10–15 users with no founder tie, to calibrate friends-inflation.
  - **Moat metric made failable:** "viewed the reflow" is demoted to hygiene
    (it's a pushed message); the real gate is **unprompted budget-surface opens**
    (≥2×/wk, not within 15 min of a log) — Strong ≥40% / Kill <20%.
- **Sunk-cost rule:** because the only gate now sits after 3 weeks of build, the
  bands must be signed **before Week 1** — by all three of us — so nobody softens
  a kill later.

## Decisions locked today (2026-07-12)

| # | Decision | Landed in |
|---|---|---|
| Day boundary | **04:00 IST** — a 2 AM log belongs to the previous day; week resets **Monday 04:00** | PRD-01 §2/§7, PRD-02 FR2 |
| Freeze rule | **Any single signal = scored day**; freeze only on total silence; no decay ever | PRD-01 FR7 |
| Unlogged sleep | `m = 1.0` neutral — missing data is never penalized | PRD-01 FR4 |
| Re-entry | ≥3 consecutive frozen days; gentle flag fires **once** | PRD-01 FR8 |
| Dual EMA | Store `ls7` + `ls30` both; **UI shows 7-day only** for now | PRD-01 §2.3/FR6 |
| HRV ratio | Clean `ĥ=0.35`; doc's Day-2 stored V corrected 94.1 → **94.0** (errata; final scores unchanged) | PRD-01 T2, doc Domain 1 §5 |
| TDEE / mass | Static onboarding values, editable in profile | PRD-01 §7 |
| Home-cook cost | ~~Per-tier ₹100/200/300~~ → **FINAL (same day, PM): flat ₹80 fully-loaded** — re-affirmed after Shreya's AOV verification `[hypothesis]` | PRD-02 FR6 + Decision record |
| Lunch baselines | ~~Raised to ₹250/350/450~~ → **FINAL (same day, PM): ₹200/300/380** (anchored under the verified ₹453–458 blended AOV) → credits **₹120/220/300** `[hypothesis on solo-lunch segmentation]` | PRD-02 FR6 + Decision record |
| Timezone | Asia/Kolkata fixed; no traveling-user handling | PRD-02 OQ5 |
| Persistence | **Supabase from day one** (auth + Postgres + RLS), not localStorage-first | build plan |
| Design order | ux-designer wireframe spec **before** code | build plan |
| Prototype scope | **Full loop, no push nudges**: dashboard, two-tap log, check-ins, Recovery Mode UI, lunchbox + savings ledger | build plan |

~~Still deliberately open: food-delivery AOV~~ — **CLOSED same day by Shreya
(`270ddc2`): national blended delivery AOV ₹453–458 FY25 [verified — Eternal +
Swiggy primary filings, GOV÷orders re-derived]**. `{suggestion}` copy ships as a
deterministic string; templatization waits for the reflow-reaction gate.

## What we need from YOU

- **Founder B (frontend):** your Week-1 input is `docs/venture/ux-spec-prototype.md`
  (5 screens, every empty/error/recovery state, tap-count audits) — being written
  now against the house heuristics. Review it before building; flag anything
  unbuildable early. Stack: Next.js App Router + Tailwind.
- **Founder C (platform):** the schema you'll own is fully specced across PRD-01 §2
  and PRD-02 §2 (`daily_inputs`, `vices_logged` with `CHECK (severity > 0)`,
  `calculated_scores` with dual-EMA columns, `weekly_budgets` with **no rollover
  column by design**, `savings_ledger`, `funnel_events` pre-auth, `consent_log`).
  Supabase project + keys needed when the scaffold starts. TWA/Play track starts
  Week 1 (the 14-day closed-test clock).
- **All three:** reply "signed" (or argue now) on the v0.4 gate bands — doc Part 2,
  Week-3 checkpoint + Week-6 gate tables. This is the pre-commitment that keeps
  the kill switch honest.

## Where everything lives

- Venture doc **v0.4**: `docs/venture/Nudge_Strategic_Documentation_Suite.md`
- Engine spec: `docs/venture/prd-01-balance-engine.md` (v1.1 — all Qs answered)
- Budget spec: `docs/venture/prd-02-budget-reflow.md` (v0.2 — all Qs answered)
- UX spec: `docs/venture/ux-spec-prototype.md` (landing today)
- Research digests (rebaseline, costs, competitor sweep): `research/2026-07-12_*.md`
- Session history: `research/journal.md` · Always-current snapshot: `STATE.md`

## Next session (any founder can run it)

Scaffold + build, per the approved plan: Next.js + TS + Tailwind + Vitest,
Supabase schema migration, then vertical slices in PRD order — every slice gated
by its PRD fixture (engine T1–T13; budget golden ₹200/day, overshoot-once,
lunchbox-₹150). The PRD worked examples are the acceptance tests.
