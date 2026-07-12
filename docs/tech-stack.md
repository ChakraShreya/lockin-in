# Tech stack decisions

Source of truth for *how* Nudge is built (the venture doc + PRDs cover *what*
it does). Read this before scaffolding, and update it — don't just append —
whenever a stack decision changes. Each entry has a date and a "why," not just
a name, so a future session (human or AI) can tell a considered choice from a
guess.

_Last updated: 2026-07-12._

---

## Decision log

| Layer | Choice | Decided | Why |
|---|---|---|---|
| Frontend framework | **Next.js (App Router) + TypeScript + Tailwind CSS** | 2026-07-12 | Matches Founder A's day-job stack (JS/TS); App Router's server/client component split doubles as the app's security boundary (secrets only ever live server-side); Tailwind avoids a separate CSS-architecture decision. |
| Backend / DB | **Supabase (managed Postgres)** | 2026-07-12 | Postgres scales from a 12-tester pilot to real usage without a re-platform; Supabase bundles auth, row-level security, and auto-generated APIs so a 3-person evenings-and-weekends team doesn't hand-roll a backend. |
| Auth | **Supabase Auth, magic link / OTP** | 2026-07-12 | No passwords to store or leak; Supabase's Next.js SSR helper manages session cookies — no hand-rolled JWT handling. |
| Authorization | **Postgres Row Level Security (RLS) on every table** | 2026-07-12 | Enforced in the database, not in application code that can forget a check — this is the app's actual DPDP/privacy guarantee (see CLAUDE.local.md rule 3), not a policy document. |
| Testing | **Vitest** | 2026-07-12 | Pairs naturally with the Next.js/TS toolchain; used to TDD the engine against the PRD-01/PRD-02 fixtures before any UI exists. |
| Hosting | **Vercel** | 2026-07-12 | Native Next.js deploys, free tier covers the pilot. |
| Distribution — phase 1 | **PWA** (installable, offline vice-log queue, web push) | 2026-07-12 | One codebase covers "the app" for the pilot; wearables/native sensors are explicitly out of MVP (PRD-01 §6), so nothing in v1 needs native APIs. |
| Distribution — phase 2 | **TWA (Trusted Web Activity) via Bubblewrap → Play Store** | 2026-07-12 | The Week 4–6 live-cohort gate requires a Play Store closed test (12 testers). A TWA wraps the deployed PWA URL in a signed AAB — no rewrite, every web deploy updates the "app" automatically. Migration effort: ~2–3 days, one-time. |
| Distribution — phase 3 (if ever needed) | **Capacitor**, only if a future feature needs a native API the web can't reach (e.g., Health Connect for wearables) | 2026-07-12 (contingency, not committed) | Same web codebase, adds a native plugin bridge; still not a full rewrite. Not needed for MVP scope. |
| Validation | **Zod schemas at every API route boundary** | 2026-07-12 | Cheap, typed, catches malformed vice-log/cost input before it reaches the DB. |
| AI integration (deferred to post-gate) | **LLM calls from Next.js server routes** (structured/tool-use output), Postgres `pgvector` extension if embeddings are ever needed | 2026-07-12 (planned, not built) | Keeps API keys server-side only; `pgvector` ships natively in Supabase so no new infra is needed if this is picked up later. Explicitly gated: not built until the Week 4–6 validation gate passes (see "Deliberately out" below). |

## Architectural principle: the engine is framework-free

`balance-engine` and `budget-reflow` (the scoring and budget-recalculation
logic from PRD-01/PRD-02) are written as **pure TypeScript modules** — no
Next.js imports, no Supabase client calls, just `(inputs) → outputs`. This is
deliberate, not incidental:

- It's the only way to TDD the math against the PRD fixtures (T1–T13, budget
  goldens) before any UI or DB exists.
- It keeps the deterministic, explainable core separate from anything AI ever
  touches later — AI wraps *around* the engine (parsing input, narrating
  output), it never computes a score or a budget number itself.
- It's portable if the distribution layer ever changes (PWA → native, or a
  different backend) without touching the logic that actually matters.

## Deliberately out (for now)

Mirrors the PRDs' own "deliberately out" convention — these are considered
and rejected *for this phase*, not overlooked:

- **Native app from scratch (React Native / Flutter)** — no MVP feature needs
  native-only APIs; revisit only if Capacitor's plugin bridge isn't enough.
- **Bank/SMS/Gmail/Account Aggregator ingestion** — verified out of reach for
  India (per venture doc Part 3); all cost/vice data is manual self-report.
- **AI-generated reflow copy / NL vice-logging** — planned (see decision log
  above) but gated behind the Week 4–6 live-cohort validation, so the core
  loop is tested with hand-written copy first.
- **Wearable (RHR/HRV) ingestion** — `Δ=0` is the shipped MVP path per PRD-01
  §6; live wearable data is a v2 regression seed only.

## Where this fits the build plan

See `STATE.md` → "Open threads / next steps" for the current phase, and the
prototype build plan referenced there for the phase-by-phase sequencing
(engine → DB/auth → screens → PWA → TWA wrap → AI layer).
