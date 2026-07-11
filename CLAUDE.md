# Project: lockin-in (Bounce)

**Bounce** — a lifestyle recalibration app: log a vice once, and both your
health plan and money reflow around it, without guilt. Personal side project,
3-founder squad. Currently pre-code: the venture doc is the source of truth.

Read [STATE.md](STATE.md) first — always-current snapshot, auto-loaded into
every session here via a SessionStart hook. [GUIDE.md](GUIDE.md) explains the
installed AI tooling (agents, flows, when to use what).

## Layout

- `docs/venture/Bounce_Strategic_Documentation_Suite.md` — **the venture doc**
  (v0.2): product spec, scoring math, validation protocol, gates, risk
  register. The single source of truth for what Bounce is.
- `docs/session-summary/` — handoff docs between sessions/founders (created on
  first `/handoff`).
- `research/` — dated research digests (`YYYY-MM-DD_topic.md`) +
  `journal.md` (append-only session history).
- `.claude/` — committed openly (personal project, no visibility constraints):
  6 role agents in `agents/`, flow skills in `skills/`, `commands/sync-context.md`.

## Conventions

- **Tests:** none yet — no code yet. When the Week-4 build sprint starts,
  add the test command here and install the `tdd` skill
  (`/onboard-project --upgrade` from `~/ai-research`).
- **Claims discipline:** every market/competitor/platform claim in venture
  docs is `[verified — source]` or `[hypothesis]` — see CLAUDE.local.md.
- End substantive sessions with **`/sync-context`**.
