# Contributing — git workflow (v1, deliberately light)

These rules apply to **all three founders and every AI session** working in this
repo. The repo is preliminary — rules will grow (coding standards, docs
standards) as the codebase does. What's here now is the minimum for a clean,
linear, three-person history.

## The rules

1. **No direct commits or pushes to `main`.** All changes land via a pull
   request into `main`. Local hooks enforce this (see setup below).
2. **Branch for everything.** Naming: `<type>/<short-desc>` —
   `feat/…` · `fix/…` · `docs/…` · `chore/…` · `research/…`
   (e.g. `docs/prd02-money-reconcile`, `feat/balance-engine-slice-a`).
3. **PRs don't block:** there is no approval mandate — self-merge is fine.
   The PR exists for visibility and history, not gatekeeping.
4. **Linear history — rebase, never merge:**
   - `git pull --rebase` always (set it once: `git config pull.rebase true`).
   - Update a branch with `git rebase origin/main`, not by merging main in.
   - Land PRs with **rebase-merge** (`gh pr merge --rebase --delete-branch`
     or the "Rebase and merge" button). No merge commits on `main`.
5. **Commit messages:** imperative subject ≤72 chars ("Add X", "Fix Y"),
   optional body explaining *why*. One logical change per commit where practical.

## One-time setup (each clone)

```sh
git config core.hooksPath .githooks   # activates the pre-commit/pre-push guards
git config pull.rebase true           # rebase on pull, never merge
```

## The hooks (`.githooks/`)

- `pre-commit` — refuses commits made directly on `main`.
- `pre-push` — refuses pushes to `main`; warns if outgoing commits include
  merge commits.

Emergency bypass (should be rare): `--no-verify`. If you find yourself using
it regularly, propose a rule change instead.

## For AI sessions (Claude Code etc.)

Same rules, spelled out in CLAUDE.md → "Git workflow". Short version:
branch → commit → `git push -u origin <branch>` → `gh pr create` →
`gh pr merge --rebase --delete-branch`. Never commit on `main`, never merge.

## Not rules (yet)

Coding standards, docs formatting/lint rules, test requirements, CI — added
when the build sprint starts. Propose additions via PR to this file.
