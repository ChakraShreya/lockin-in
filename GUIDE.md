# GUIDE — AI tooling in this repo (and when to use what)

This repo was onboarded from `~/ai-research` (STANDARD tier + BUSINESS bundle,
2026-07-11). This guide is the manual. Source of truth for the templates is
`~/ai-research/tooling/` — if something here proves broken or better, fold the
fix back there (the project copy is a deployment, not the original).

---

## The 30-second version

- **Every session auto-loads [STATE.md](STATE.md)** — you start with full
  context, no re-explaining. **End substantive sessions with `/sync-context`**
  so the next session does too.
- **The product is the venture doc** (`docs/venture/`). The agents below exist
  to research it, stress-test it, revise it, and turn it into buildable plans.
- **One rule above all — claims discipline:** every market/competitor/platform
  claim is `[verified — source]` or `[hypothesis]`. Gemini pastes and blog
  claims are unverified input until `claim-verifier` passes them.

## The venture pipeline (the route)

```
idea/change ─► grill ─► /venture-research ─► claim-verifier ─► /venture-plan ─► gate review ─► dev handoff
              (align)   (facts, fan-out)     (kill-pass)       (doc revision)   (vs pre-set    (PRD + slices,
                                                                                 bands)         Week-4+)
```

Full route with invariants: `~/ai-research/workflows/venture-pipeline.md`.

## The six role agents (`.claude/agents/`)

Ask for them by name ("have ceo-strategist look at…") or let the flows drive
them. Each has a scope fence — if an agent refuses, that's by design; use the
right one.

| Agent | Use for | Won't do |
|---|---|---|
| **ceo-strategist** | proceed/kill calls, positioning, gate design; ends with a recommendation + riskiest assumption | write plans, specs, or copy |
| **product-manager** | PRDs, phased plans with gates + kill switches, MVP fences, dev handoff | re-decide strategy, write code |
| **marketing-growth** | positioning statements, GTM/channels, pricing framing, copy (3 variants) | scope features, fake numbers |
| **ux-designer** | screen-by-screen critique vs house heuristics, text wireframe specs | write code/CSS, cut scope |
| **market-researcher** | any outside-world question; outputs a tagged Research Digest table | recommend strategy, untagged claims |
| **claim-verifier** | checking Gemini pastes, external stats, and the doc's math; Confirmed / Contradicted / Unverifiable | soften a verdict because it's load-bearing |

House UX heuristics (baked into ux-designer/design-critique): vice log ≤2 taps
& <3s · supportive tone in every state, never guilt-based · wearable-free
default · casual view shows only Life Score + budget.

## The flows (slash commands / skills)

| Command | What it does | Reach for it when |
|---|---|---|
| `/venture-research <topic>` | Fans out 2–4 market-researchers on non-overlapping question sets, kill-passes load-bearing claims through claim-verifier, writes a dated Research Digest to `research/` | A decision hangs on several unknowns (competitor sweep, price audit, platform rules). Token-heavy — for one small question, just ask market-researcher directly |
| `/venture-plan <idea or doc path>` | Grill → ceo-strategist → marketing-growth → product-manager → one versioned venture doc | Revising the Bounce doc (v0.2 → v0.3), or planning a new phase properly |
| `/design-critique <section/flow>` | ux-designer critiques the named surface vs house heuristics; validated findings + prioritized fix list | Before building any screen, and after any Domain 2 edit |
| `grilling` (auto-triggers, or say "grill me") | One-question-at-a-time alignment until shared understanding | Before committing to any plan or doc revision |
| `/handoff <next focus>` | Compacts the session into `docs/session-summary/` | Ending mid-task, or passing work to another founder |
| `/sync-context` | Appends journal entry + refreshes STATE.md | End of every substantive session — non-negotiable habit |

## Session rhythm

1. Open a session — STATE.md loads automatically (check it's in the preamble).
2. Work. Delegate mechanical doc-scans to a haiku subagent
   (pattern in CLAUDE.local.md); keep judgment on the session model.
3. External research arrives (Gemini etc.)? → claim-verifier **before** it
   touches the doc.
4. Done → `/sync-context`. Handing off → `/handoff` too.
5. Git only when you explicitly ask for it (CLAUDE.local.md rule 1).

## What's deliberately NOT installed (yet)

- **tdd skill** — no code, no test command. At the Week-4 build sprint, run
  `/onboard-project /Users/aryankumar/lockin-in --upgrade` from an
  `~/ai-research` session; it installs the delta (tdd, optionally FULL-tier
  usage-logging hooks + two-axis-review for the implementation phase).
- **cfo-analyst / compliance-researcher agents** — don't exist yet anywhere;
  until then claim-verifier re-derives financial math, and regulatory
  questions are a market-researcher question set.

## Current known issues (as of onboarding)

Live list is in STATE.md → "Open failures"; headline: the doc's 24–48h
rebaseline claim needs a real source, and Domain 2 has no empty/error/recovery
states. Fix these before trusting either section downstream.
