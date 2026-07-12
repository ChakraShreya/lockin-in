# Research Journal

Append-only, dated history of work on `lockin-in` (Nudge). Newest entries on
top. The always-current snapshot is [../STATE.md](../STATE.md); this is the
trail of how we got there.

Maintained by `/sync-context` — each run prepends one entry. Format per entry:
**What we did · What we concluded · How it's going / next.**

---

## 2026-07-12 (evening meeting follow-up) — budget goes MONTHLY; meeting ideas triaged

**What we did**

- Triaged the 8 ideas from the founders' evening meeting (`founder_discussions`
  notes) against the PRDs: 2 sprint adds, 4 backlog, 1 absorbed, 1 major change.
- **Executed the major change — budget period weekly → monthly** (founder
  decision, all three): PRD-02 bumped to **v0.4** (`monthly_budgets` schema,
  1st-of-month 04:00 IST boundary, pro-rated first month, reworked fixtures +
  §8 scripts, group-order note in FR4, auto bill-splitting fenced out, new
  **FR8 weekly pacing checkpoint**). UX spec bumped to **v1.1** (all "till
  Monday"/weekly copy → monthly + pacing line in Advanced Metrics). Venture doc
  bumped to **v0.4.1** (Domain 1 §6, loop diagram, Part 3 schema, glossary,
  copy-probe message).
- Founder A decisions folded in same session: monthly tier amounts
  ₹6,500/13,000/26,000 **confirmed-for-now** (retunable, still `[hypothesis]`);
  **pro-rating decided**; **FR8 signed off** (Slice F now unconditional).
- Process note: the product-manager subagent's first PRD-02 write reported
  success but never persisted to disk — caught by verifying the file, re-ran,
  then verified content directly. Verify subagent writes on disk.

**What we concluded**

- Meeting-idea triage: **staple-diet presets** (#6) and **group-order
  share-editing** (#2, via FR4) are sprint-worthy; **BCA report tracking** (#1,
  mechanism undecided — manual entry recommended over upload for DPDP),
  **per-user high/low calibration** (#4/#7) and **sex-based score adjustment**
  (#8, engine tunable only; avoid blackout/medical framing) are backlog;
  weekend-blowoff worry (#3) is absorbed by the monthly pot + FR8 pacing.
- The weekly reset was doing anti-guilt work; FR8's soft weekly pace read
  (derived-on-read, no reset, no alarm) preserves that cadence under a monthly pot.

**How it's going / next**

- PRD-02 v0.4, UX spec v1.1, venture doc v0.4.1 all uncommitted in the working
  tree (user commits explicitly). Staple-presets FR still needs writing
  (PRD-01/UX spec). Gate-bands sign-off (#4) and BCA mechanism (#5) deferred
  to a later founder call.

## 2026-07-12 (later night) — product renamed Bounce → Nudge

**What we did**

- Founder decision: the product is renamed from **Bounce** to **Nudge**.
  GitHub repo renamed manually by the user to `github.com/ChakraShreya/nudge`
  (local `origin` remote URL not yet updated — flagged, needs an explicit ask
  per CLAUDE.local.md rule 1).
- Swept all 22 files containing "Bounce" (79 occurrences, found via an Explore
  pass) and replaced the product name with "Nudge" throughout: venture doc,
  both PRDs, UX spec (including its dashboard mockup label), research digests,
  STATE/CLAUDE/GUIDE/README, all 6 agent personas, all 3 relevant skills, the
  sync-context command, and the alignment handoff doc.
- **The venture doc's named recovery mechanic, "The Bounce"** (§4, "the name is
  the mechanic"), was also renamed — to **"The Rebound"** — since leaving it
  as "Bounce" would orphan-reference the old product name. Updated in 5 spots
  across the venture doc and PRD-01 (test-case names, worked-example headings,
  the comparison-table cell, the canonical mechanic definition).
- **Renamed the venture doc file itself:**
  `Bounce_Strategic_Documentation_Suite.md` →
  `Nudge_Strategic_Documentation_Suite.md` (via `git mv`, preserving history),
  and updated the ~13 files that referenced it by path.

**What we concluded**

- Verified zero residual "bounce" (case-insensitive) references anywhere in
  the repo after the sweep.

**How it's going / next**

- Working tree has the rename staged as an `RM` (rename) on the venture doc
  plus modifications everywhere else; not committed (git only on explicit ask).
  When committing: branch → PR → rebase-merge, per the now-adopted workflow.
- **Follow-up needed:** update the local `origin` remote URL to
  `github.com/ChakraShreya/nudge.git` when asked.

---

## 2026-07-12 (night) — pull review, money reconciliation, git workflow

**What we did**

- Reviewed pulled commits: Shreya's `270ddc2` **verified the food-delivery AOV**
  (₹453 Eternal / ₹458 Swiggy, FY25 primary filings, GOV÷orders re-derived and
  re-checked here) — genuinely closes OQ1 — but her PRD-02 v0.3 was drafted from
  stale text and re-asserted the retired flat-₹80/₹200-300-380 constants,
  leaving the PRD internally contradictory with the same-day per-tier call.
- **Founder A's final call: Shreya's verified-anchor numbers WIN** — flat ₹80
  home cost, ₹200/300/380 lunch baselines, credits ₹120/220/300. Reconciled as
  PRD-02 **v0.3.1** (consistent table/examples/fixtures/script; Monday 00:00
  slip → 04:00; honest version history; Decision-record table added where §9
  was deleted). Downstream synced: venture doc lunchbox line, UX-spec note,
  handoff decisions table, STATE.
- **Git workflow adopted** (founder rule): PRs-only into main, feature branches,
  rebase-not-merge for linear history — CONTRIBUTING.md + `.githooks/`
  (pre-commit/pre-push block direct main commits) + PR template + CLAUDE.md
  section so AI sessions comply. Deliberately light; rules grow with the repo.

**What we concluded**

- Cross-session doc conflicts are now a real failure mode (two founders' AI
  sessions edited the same PRD hours apart). Mitigations: the PR workflow, plus
  decision-record tables in PRDs so decisions can't be silently overwritten by
  stale drafts.

**How it's going / next**

- Landed via PRs `docs/prd02-money-reconcile` + `chore/git-workflow`
  (rebase-merged). Next session: the prototype build (scaffold + slices) — on a
  branch, per the new workflow.

---

## 2026-07-12 (evening) — all PRD decisions made; team alignment handoff

**What we did**

- Founder A answered **all 12 open PRD questions** via guided Q&A (one batch at a
  time, recommendations first). Landed in PRD-01 v1.1 (§7 = decision table) and
  PRD-02 v0.2. Highlights: **04:00 IST day boundary** (week resets Monday 04:00),
  any-signal=scored, m=1.0 on null sleep, dual EMA stored / 7d shown, ĥ=0.35
  pinned (doc Day-2 errata 94.1→94.0), TDEE static, **home cost per-tier
  ₹100/200/300** (founder's own numbers), **lunch baselines raised to ₹250/350/450
  → credit ≈₹150/day all tiers** (supersedes doc's ₹120–300 range).
- Prototype build plan approved (design → build; friends & family = test cohort
  later): Supabase from day one, ux-designer wireframes first, full loop minus
  push nudges. Plan: `~/.claude/plans/research-on-the-current-reflective-bear.md`.
- ux-designer writing `docs/venture/ux-spec-prototype.md` (5 screens, all
  empty/error/recovery states) — Founder B's build input.
- **Founder alignment handoff written** (today's stated main objective):
  `docs/session-summary/2026-07-12_decisions-and-alignment.md` — TL;DR, decisions
  table, per-founder asks, gate-band sign-off request.

**What we concluded**

- Mid-session priority call by Founder A: today = decision-complete PRDs + everyone
  on the same page; the scaffold/build itself moves to the next session.
- The ₹150-flat lunchbox credit is a deliberate trade: founder-realistic home
  costs (₹100/200/300) squeezed credits, so baselines were raised rather than
  letting the savings story die — all six constants remain [hypothesis].

**How it's going / next**

- Both PRDs are build-ready with zero open blocking questions (only OQ1 delivery
  AOV stays open, accepted as [hypothesis]).
- Next session: scaffold (Next.js + TS + Tailwind + Vitest + Supabase schema) and
  build vertical slices gated by PRD fixtures. Needs from Founder A then:
  Supabase project URL + anon key into `.env.local`.
- Founders B & C: read the handoff; sign the v0.4 gate bands.

---

## 2026-07-12 (later) — pilot dropped; build-first protocol (doc v0.4)

**What we did**

- **Founder decision: the WhatsApp concierge pilot is dropped.** Chosen follow-on:
  build → live-cohort gates (not a lighter substitute, not gateless).
- ceo-strategist redesigned the validation protocol; landed as **doc v0.4**:
  Phase 0 (wk 0) → build sprint (wks 1–3, old Phase-2 data work folded in) →
  Week-3 closed-test checkpoint (n=12, funnel+activation, fix-before-spend) →
  live cohort (wks 4–6) as the first and only decision gate. 9→7 weeks.
- Reconciled PRD-02: §8 concierge script marked obsolete (retained as Week-5
  interview guide + copy source); gate table swapped to the v0.4 metrics.
- Amended the forward plan file (Track 2 superseded).

**What we concluded**

- **The moat now ships unvalidated** — new riskiest assumption: users want the
  prospective ₹ number at all; first evidence arrives Week 4+ in-app, not Week 2
  in chat. Clawbacks: ₹0 recruitment-embedded copy probe (wk 0), Week-3
  checkpoint, cold arm (10–15 no-founder-tie users) as the inflation instrument,
  Week-5 interviews (n≥6).
- **Loop-engagement metric split:** reflow delivery-view demoted to hygiene
  (≥80%, pushed messages are near-automatic to see); the real moat test is
  **unprompted budget-surface opens** (≥2×/wk, not within 15 min of a log),
  Strong ≥40% / Kill <20% [hypothesis bands].
- Surveillance risk raised to High (its Phase-1 mitigation died); new risks:
  sunk-cost pressure at the only gate (mitigation: all three founders sign the
  v0.4 bands **before Week 1**) and untested nudge copy.

**How it's going / next**

- Doc v0.4 + PRD-02 reconciled, not yet committed (user's call).
- **Human next:** all 3 founders sign the v0.4 gate bands before Week 1; PRD
  open-question sign-off; recruitment (with copy probe) + DPDP — recruitment is
  now the critical path.
- **AI next:** Domain 2 UX state specs + Recovery Mode spec (was already queued;
  now feeds a Week-1 build start), then nudge spec + copy v0.

---

## 2026-07-12 — v0.3 claim-hardening + forward plan

**What we did**

- Set the forward direction: **parallel tracks** — deepen all six features into
  build-ready specs while Phase 0/1 pilot prep runs alongside; pilot stays the
  gate. Plan at `~/.claude/plans/research-on-the-current-reflective-bear.md`.
- Ran a 3-way research fan-out (market-researcher): rebaseline source hunt,
  Bangalore vice-cost audit, closed-loop competitor sweep → three digests in
  `research/2026-07-12_*.md`.
- Kill-passed the load-bearing findings through claim-verifier, then hardened
  the venture doc to **v0.3**.

**What we concluded**

- **Rebaseline claim re-sourced, not just downgraded.** Oura has no
  recovery-timeframe data (its numbers are same-night deltas). claim-verifier
  found the source the researcher missed — **MunichBREW II (Eur Heart J 2024)**:
  ~24h HRV/HR normalization under *heavy dosing*, but WHOOP real-world shows a
  4–5-day tail. Now `[hypothesis]`; the 48h Recovery window is a tunable
  placeholder, not a guarantee. RHR corrected to *lowest* RHR +8.2%.
- **The moat survives the sweep** — but sharpened: Nudge's edge is the
  **prospective re-plan** (one log → forward recalculation of both plans), not
  dual display. Reframe/Sunnyside/DrinkControl already show money+calories
  retrospectively. Paceline / Aditya Birla = adjacent-but-opposite (exercise→money).
- **Smoking costs stale** — Feb 2026 excise overhaul [verified]; updated.
  Alcohol/food-delivery/lunchbox costs only partially auditable — food-delivery
  AOV unverifiable from primary sources (investor PDFs are image-only); flagged.

**What we did (cont.)**

- Started Track 1 (feature deepening): product-manager wrote **PRD-01 Balance
  Engine** (`docs/venture/prd-01-balance-engine.md`) and **PRD-02 Budget Reflow**
  (`docs/venture/prd-02-budget-reflow.md`) — Founder A's Week-4 lane. Both pin
  the doc's worked examples as exact test fixtures and spec the previously
  under-specced edge cases.

**How it's going / next**

- v0.3 landed; both onboarding-era doc findings closed. PRD-01/02 drafted.
- **Flags to resolve** (open questions in the PRDs): doc Day-2 example uses
  ĥ≈0.354 for −35% HRV (should be 0.35) — pin the HRV-ratio definition;
  schema needs a `daily_inputs` input table + dual ls7/ls30 columns (PM added,
  needs sign-off); ₹80 home-cost resolved to *fully-loaded* [hypothesis];
  food-delivery AOV still unverifiable (do not upgrade to [verified]).
- Next per the plan: Domain 2 UX state specs (`/design-critique` → ux-designer),
  then Recovery Mode spec, then nudge spec + copy. Not committed (user's call).

---

## 2026-07-11 — onboarded via ai-research

**What we did**

- Onboarded this repo at tier **STANDARD + BUSINESS bundle** using ai-research
  tooling: context capture (STATE/journal/sync-context/SessionStart hook),
  grilling + handoff skills, 6 business role agents, 3 venture flows, GUIDE.md.
- Copied the Nudge venture doc v0.2 into `docs/venture/`.
- Smoke-tested claim-verifier against the doc: Oura figures, Play fee/testing
  rules, and Day-1 math all confirmed; **"24–48h rebaseline" contradicted**
  (not in the cited Oura source).

**What we concluded**

- STANDARD (not FULL) because no code/tests exist yet — upgrade at Week-4
  build sprint. BUSINESS because this is a venture repo: the doc is the
  product right now.
- tdd skill deliberately skipped (no test command) — install with `--upgrade`.

**How it's going / next**

- Run /sync-context after the first real session. First real work: fix the
  two doc findings (STATE.md → Open failures), then Phase 0.
