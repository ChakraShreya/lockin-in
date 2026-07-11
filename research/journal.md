# Research Journal

Append-only, dated history of work on `lockin-in` (Bounce). Newest entries on
top. The always-current snapshot is [../STATE.md](../STATE.md); this is the
trail of how we got there.

Maintained by `/sync-context` — each run prepends one entry. Format per entry:
**What we did · What we concluded · How it's going / next.**

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
