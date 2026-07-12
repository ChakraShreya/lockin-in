# Team, current knowledge, and learning path

Context for any session (human or AI) on **who's building Nudge, what they
already know, what they don't yet, and how the team wants to learn** — so
guidance gets pitched at the right level instead of assuming either total
beginners or working familiarity with this specific stack. Update this file
as knowledge actually shifts (don't let it go stale — it should read true,
not aspirational).

_Last updated: 2026-07-12._

---

## The founders

3-person side-project squad, evenings and weekends. All three are junior
developers by day job, and **all three are starting from ~zero on this
specific stack** (Next.js, Supabase/Postgres, PWA/TWA distribution) — day-job
experience is adjacent, not directly transferable, and none of it should be
assumed as prior knowledge when explaining this project's code.

- **Founder A** — engine/PM lane per the venture doc's Part 4 charter.
  Day job: junior SDE (JavaScript, Spring Boot, Java, TypeScript). Closest
  existing skill to this stack (JS/TS), but no React/Next.js or Postgres
  experience yet.
- **Founder B** — Day job: junior Python developer, working on optimizing AI
  workflows. Strong Python/AI-workflow instincts; JS/TS and SQL are new.
- **Founder C** — Day job: junior Python developer, works on AI and
  cybersecurity. Security instincts transfer well (useful for the RLS/auth
  work); JS/TS, React, and SQL are new.

None of the three has shipped a production web app, used Postgres, or built
a PWA/TWA before this project. Treat every stack concept below as genuinely
new material, not a refresher.

## What "success" means for this project (as agreed 2026-07-12)

Two goals, held simultaneously, neither traded off against the other:

1. **Ship and validate the product** — execute the venture doc's v0.4
   build-first protocol through the Week 4–6 live-cohort gate.
2. **All three founders come out the other side competent across the whole
   stack** — not just "the app got built," but each person can read, explain,
   and modify any layer of it, with one area each knows deepest.

## How we want to learn

- **Start together, specialize later.** Because knowledge is near-zero across
  the board right now, Week 0 and the first two build phases (engine, DB/auth)
  are done as a group — pairing/mobbing, rotating who drives — rather than
  splitting up immediately. Splitting too early with no baseline means nobody
  can review anyone else's work.
- **Learn by breaking things on purpose**, not just reading docs — e.g. the
  Week 0 exercise of turning RLS off, observing the data leak, then turning it
  on and writing the fix. Concepts that are demonstrated as a failure-then-fix
  stick better than descriptions of "why security matters."
- **Rotating primary ownership after the shared foundation**, so specialization
  emerges from real reps rather than being assigned upfront by day-job title.
  Current rotation plan (subject to change as it's tried):

  | Phase | Primary | Shadow(s) |
  |---|---|---|
  | Engine (pure TS, PRD fixtures) | Founder A | B & C review, must explain why |
  | DB + Auth + RLS | Founder C | A & B each write a policy themselves |
  | Screens (UI wiring) | Founder A, paired rotating with B/C | remaining founder |
  | PWA (offline queue, service worker) | Founder B | A & C |
  | TWA + Play Store publish | Rotates — whoever hasn't shipped yet | — |
  | AI layer (post-validation-gate) | Founder B or C | Founder A |

- **Every PR needs a shadow-reviewer who can explain the "why," not just
  approve that it runs** — a short walkthrough from the primary owner before
  merge, especially in the first few PRs of each phase.
- **Pace is honest, not aspirational.** A genuine Week 0 foundations sprint
  (React → Next.js → SQL/Supabase → auth/RLS, hands-on) pushes the Week 3
  build-sprint checkpoint to roughly Week 4. This was flagged and accepted
  2026-07-12 rather than silently compressed.

## Learning path (sequenced, mapped to build phases)

**Week 0 — shared foundations (mob/pair, no solo work yet)**
1. React fundamentals (components, state, props) — react.dev "Learn React."
2. Next.js on top of React (routing, server vs. client components — the
   security boundary of the whole app) — Next.js official "Learn" course.
3. SQL basics if rusty (sqlbolt.com) + Supabase "Getting Started" — build one
   throwaway table, query it from a Next.js page.
4. Auth + RLS, hands-on: build Supabase magic-link auth into the throwaway
   project; deliberately break RLS (off → data leak visible), then fix it
   (on → policy written, leak blocked).

**Weeks 1–3+ (paired, rotating per the ownership table above)**
1. Engine — TypeScript, Vitest, turning a written spec (PRD-01/PRD-02) into
   tested code.
2. DB + Auth — SQL migrations, writing real RLS policies, Supabase auth flows
   end to end.
3. Screens — React state/forms, wiring engine + DB into real UI.
4. PWA — service workers, manifests, offline-first patterns (new to everyone).
5. TWA + Play Store — signing, publishing pipeline (one-time, low-complexity,
   high learning value — rotate to whoever hasn't shipped anything yet).
6. AI layer (post Week 4–6 gate only) — structured LLM output, prompt design
   for the reflow copy, closest to B/C's day-job strength.

See `docs/tech-stack.md` for what each layer *is* and why it was chosen, and
`STATE.md` for which phase is currently active.
