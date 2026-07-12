# Research Journal

Append-only, dated history of work on `lockin-in` (Bounce). Newest entries on
top. The always-current snapshot is [../STATE.md](../STATE.md); this is the
trail of how we got there.

Maintained by `/sync-context` — each run prepends one entry. Format per entry:
**What we did · What we concluded · How it's going / next.**

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
- **The moat survives the sweep** — but sharpened: Bounce's edge is the
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
- Copied the Bounce venture doc v0.2 into `docs/venture/`.
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
