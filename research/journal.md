# Research Journal

Append-only, dated history of work on `lockin-in` (Bounce). Newest entries on
top. The always-current snapshot is [../STATE.md](../STATE.md); this is the
trail of how we got there.

Maintained by `/sync-context` — each run prepends one entry. Format per entry:
**What we did · What we concluded · How it's going / next.**

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
