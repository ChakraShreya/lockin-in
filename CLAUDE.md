# Project: lockin-in (Nudge)

**Nudge** — a lifestyle recalibration app: log a vice once, and both your
health plan and money reflow around it, without guilt. Personal side project,
3-founder squad. Currently pre-code: the venture doc is the source of truth.

Read [STATE.md](STATE.md) first — always-current snapshot, auto-loaded into
every session here via a SessionStart hook. [GUIDE.md](GUIDE.md) explains the
installed AI tooling (agents, flows, when to use what).

## Layout

- `docs/venture/Nudge_Strategic_Documentation_Suite.md` — **the venture doc**:
  product spec, scoring math, validation protocol, gates, risk register. The
  single source of truth for what Nudge is (version history at its foot).
- `docs/session-summary/` — handoff docs between sessions/founders (created on
  first `/handoff`).
- `research/` — dated research digests (`YYYY-MM-DD_topic.md`) +
  `journal.md` (append-only session history).
- `.claude/` — committed openly (personal project, no visibility constraints):
  6 role agents in `agents/`, flow skills in `skills/`, `commands/sync-context.md`.

## Conventions

- **Tests:** none yet — no code yet. When the build sprint starts,
  add the test command here and install the `tdd` skill
  (`/onboard-project --upgrade` from `~/ai-research`).
- **Claims discipline:** every market/competitor/platform claim in venture
  docs is `[verified — source]` or `[hypothesis]` — see CLAUDE.local.md.
- End substantive sessions with **`/sync-context`**.

## Git workflow (applies to AI sessions — full rules in CONTRIBUTING.md)

- **Never commit or push on `main`** — PRs only. Local `.githooks/` enforce this
  (activate once per clone: `git config core.hooksPath .githooks`).
- Flow: `git checkout -b <type>/<short-desc>` (feat/fix/docs/chore/research) →
  commit → `git push -u origin <branch>` → `gh pr create` →
  `gh pr merge --rebase --delete-branch`. No approval mandate; self-merge is fine.
- **Linear history:** `git pull --rebase` always (`git config pull.rebase true`);
  rebase branches on `origin/main`, never merge main into a branch; PRs land via
  rebase-merge only.
- When to run git at all is still governed by CLAUDE.local.md (only when
  explicitly asked).
