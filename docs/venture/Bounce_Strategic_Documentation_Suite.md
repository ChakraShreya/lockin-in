# PROJECT BOUNCE — VENTURE DRAFT
## The Closed-Loop Lifestyle Recalibration Engine
### Working document: desk-research validated, pilot pending

---

**Document Version:** 0.4 (Build-first — WhatsApp pilot dropped, live cohort is the first gate)
**Founding Team:** 3-Developer AI-Augmented Squad (side project)
**Prepared:** July 2026
**Status:** Desk research done (July 2026), core whitespace verified (sweep 2026-07-12), math refactored. **Nothing is validated with real users yet** — founder decision 2026-07-12: the WhatsApp pilot is dropped; **the Week-4–6 live cohort is the first real evidence gate**, with a Week-3 closed-test checkpoint and a ₹0 recruitment copy probe as the only pre-code signals. Tweak freely.

---

> **How to read this document**
> v0.1 was pure hypothesis. v0.2 folds in verified market research (sources cited in the Research Digest below) and a refactored, internally-consistent scoring model. Claims marked **[verified]** trace to a real source; everything else — especially every severity weight, point cap, and threshold in the math — is a **calibration placeholder** to be tuned with pilot data. The plan in Part 2 is the current best sequence; it is designed to be cheap to abandon at every gate.

---

## EXECUTIVE SUMMARY

**Bounce** is a lifestyle recalibration app built on one mechanic: **log a vice once, and both your health plan and your money reflow around it — without guilt.**

A user logs "Philosophical night at Toit" in two taps. Bounce then:
1. **Absorbs it into a rolling Life Score** (a smoothed portfolio, not a daily pass/fail) — one bad night can never cost more than a bounded fraction of the score;
2. **Reflows the week's discretionary budget** — "₹600 left for fun this week, here's the cheapest path back";
3. **Adjusts the next 48 hours** — Recovery Mode trims training targets, raises hydration/protein emphasis, and the UI softens.

### Why this and not just another wellness app

Competitive research (July 2026) found:

- **The closed loop is genuinely unoccupied [verified — store + Crunchbase/Tracxn sweep, 2026-07-12].** No app, globally or in India, takes a logged vice and *forward-recalculates* both health targets and budget. Sharpened by the sweep: a few trackers (Reframe, Sunnyside, DrinkControl) already show money-saved *and* calories from one drink log — but as **retrospective dashboards**, not a **prospective re-plan**. The forward recalculation is the actual moat. Adjacent-but-opposite closed loops exist and are fundable — Paceline (US) and Aditya Birla Activ Health (India) both run exercise→money — validating the mechanic without occupying it. Indian incumbents (HealthifyMe, Cult.fit, Jupiter, CRED) run parallel scoring systems that never bridge domains.
- **"No-shame" tone alone is crowded [verified].** Reframe, Sunnyside, Hoot, and Finch already do guilt-free logging well. Bounce's differentiation is the **recalibration mechanic**, not the empathy framing. Every pitch sentence should lean on "one log adjusts everything," not "we don't judge you."

### Positioning (hypothesis)

| Dimension | Incumbents | Bounce |
|---|---|---|
| **Core mechanic** | Track & display (silos: health *or* money) | One log → dual recalculation (health *and* money) |
| **Failure handling** | Streak breaks, guilt loop | Bounded damage + visible path back ("Bounce") |
| **Vice scope** | Alcohol-only (Reframe/Sunnyside) or food-only (MFP) | Alcohol, smoking, food delivery, sugar — India-calibrated |
| **Data demands** | Weighing food, wearables assumed | Two-tap tiers, household units, wearable-free core |

**Target market (unchanged):** urban tech workers 22–35 + college students, Bangalore first. Most-drinking-relevant cohort is urban Gen Z (participation rising), but the broadened vice set (delivery food, sugar) serves the abstinent-but-health-conscious majority.

---

## RESEARCH DIGEST (July 2026) — what we verified before writing v0.2

| Finding | Verdict | Consequence in this doc |
|---|---|---|
| Closed-loop spend↔physiology whitespace | **Confirmed — sweep done** [verified — App/Play-store keyword + Crunchbase/Tracxn/Product Hunt sweep, 2026-07-12]. Nothing recalculates *both* a forward health plan *and* a forward budget from one vice log. Sharpening: several trackers (Reframe, Sunnyside, DrinkControl) already show money-saved + calories side-by-side from one drink log, but as **retrospective dashboards**, not **prospective re-planning** — Bounce's real moat is the *forward recalculation*, narrower and better-defended than "no dual-domain app exists." | Loop promoted to core mechanic (Part 1); pitch the *forward re-plan*, not merely dual display |
| "No-shame" framing as differentiator | **Crowded** — Reframe, Sunnyside, Hoot, Finch | Differentiate on mechanic, not tone |
| RBI Account Aggregator for bank data | **Structurally out of reach** — FIU must be RBI/SEBI-regulated; no TSP bypass; ₹2 Cr NOF for own license | Manual logging is the hero trigger; AA deferred to funded-company stage, permanently out of MVP |
| Gmail transaction parsing | **Blocked** — restricted-scope API needs $15–75k CASA assessment | Not pursued |
| SMS transaction parsing | **Viable but risky** — must be disclosed core function; Play Store rejections documented (Bluecoins); Android-only | v2 candidate, not MVP |
| PWA push notifications on Android | **Works** — full Push/Notification API support in Chrome | Nudge engine technically sound |
| PWA-only distribution | **False economy** — Play Store = $25 one-time + free Bubblewrap TWA wrap; 12-tester/14-day closed-testing rule satisfied by pilot cohort itself | Ship PWA **and** TWA from day one |
| Unknown: link→install→notification-permission funnel | **Unmeasured anywhere** — biggest silent-failure risk | Promoted to a primary pilot metric |
| D7 ≥45% success gate (v0.1) | **Miscalibrated** — category median D7 ≈ 7–20%; good = 28–40%; 45% = Duolingo-tier | Gates reset: kill <20%, iterate 20–30%, strong ≥30% |
| India wearables assumption | **Whoop/Oura = premium niche** — smart rings −30.6% in 2025; Oura India only since Mar 2026 at ₹29k+; Android ≈95% | Wearable-free core mandatory; Health Connect primary, Ultrahuman candidate API |
| Alcohol physiology anchor (same-night deltas) | **Oura 600k-member data [verified — ouraring.com]:** HRV −15.6%, **lowest** RHR +8.2% (source's own label — this is the lowest-RHR metric, *not* average RHR). Same-night/adjacent-day deltas only — the Oura source carries **no** recovery-timeframe data. | Used to calibrate ω (the physiology top-up) |
| Rebaseline window (how long recovery takes) | **Re-sourced [hypothesis — MunichBREW II, Eur Heart J 2024]:** the originally-cited Oura study never established this. MunichBREW II (48h continuous ECG, n=202, BAC ≥1.2 g/kg) shows HR/HRV normalizing by ~24h — but *under controlled heavy dosing*, and WHOOP real-world data shows 19–29% still suppressed at day 2–3 with a tail to day 4–5. | Treat 24–48h as an **optimistic median, not a design guarantee**; the 48h Recovery window is a calibration placeholder, tunable with pilot wearable data |
| DPDP Act 2023 burden at our scale | **Manageable** — privacy policy, named grievance officer, consent logging; no consent-manager requirement for tiny apps | Added to MVP checklist |

*Key sources: Paceline (paceline.fit, CNBC), Reframe/Sunnyside/Hoot product pages, IDC India wearables 2025, CASParser State-of-AA-2026, Setu AA pricing, Google Play SMS-permission policy + Bluecoins case, Google Play closed-testing help, Oura alcohol-impact study (600k members), UXCam/Business-of-Apps retention benchmarks, dpdpa.com. Full links in research session notes.*

---

---

# PART 1: PRODUCT SPECIFICATION — Draft Concept

*Starting hypotheses, sharpened by research. Argue with everything; the pilot decides.*

## 1.1 The Core Loop (the product in one diagram)

```
                    ┌──────────────────────────────┐
                    │   VICE LOGGED (two taps)     │
                    │  type + tier (+ cost, opt.)  │
                    └──────────────┬───────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          ▼                        ▼                        ▼
 ┌─────────────────┐    ┌──────────────────┐    ┌──────────────────┐
 │  LIFE SCORE     │    │  BUDGET REFLOW   │    │  48H RECOVERY    │
 │  EMA absorbs    │    │  week's fun-money│    │  strain −25%,    │
 │  bounded hit    │    │  recalculated    │    │  +hydration,     │
 │  (≤25%/day)     │    │  per day left    │    │  soft UI         │
 └─────────────────┘    └──────────────────┘    └──────────────────┘
          │                        │                        │
          └────────────────────────┼────────────────────────┘
                                   ▼
                    "Logged. Score absorbed it, ₹200/day
                     left till Monday — here's the path back."
```

No incumbent runs all three arrows from one input **[verified whitespace]**. That loop *is* the product; everything else is delivery.

## 1.2 Broadened Vice Taxonomy (India-calibrated)

Four domains, three tiers each, logged qualitatively — never as raw numbers:

| Domain | Tier 1 | Tier 2 | Tier 3 |
|---|---|---|---|
| **Alcohol** | `Buzzed` — **10** | `Philosophical` — **25** | `Blackout / Never Again` — **50** |
| **Smoking** | `Sutta-Chai Break` — **8** | `Drunk Cigs Don't Count` — **18** | `Too Stressed, Smoked a Pack` — **40** |
| **Food delivery / cheat** | `Mindful Order` — **5** | `Late-Night Binge` — **15** | `Weekend Write-Off` — **30** |
| **Sugar** | `Extra-Sweet Chai` — **3** | `Dessert Raid` — **8** | `Sugar Spiral` — **15** |

**Calibration status:** alcohol severities are loosely anchored to real physiology (Oura 600k data [verified]: a drinking night ≈ HRV −15.6%, **lowest** RHR +8.2% — the heaviest measurable vice signature, hence the heaviest tiers; recovery *duration* is a separate, weaker claim — see the Rebaseline row in the Research Digest). Smoking/food/sugar weights are **relative guesses** pending pilot + wearable-delta data. Severities are stored as **positive magnitudes**; the formula's minus sign applies the penalty (storing negatives silently flips penalties into bonuses — a bug we already hit once).

**Default cost per tier** (editable at log time; drives the budget reflow):

| Domain / Tier | ₹ (budget) | ₹₹ (mid) | ₹₹₹ (premium) |
|---|---|---|---|
| Alcohol T1 / T2 / T3 | 300 / 600 / 1000 | 600 / 1200 / 2500 | 1200 / 2500 / 5000 |
| Delivery T1 / T2 / T3 | 200 / 250 / 500 | 400 / 500 / 900 | 800 / 900 / 1600 |
| Smoking (per unit/pack) | **Updated** [verified — TaxGuru excise notification + live Zepto retail, 2026-07]: a **Feb 1 2026 excise overhaul** (₹2,050–8,500 / 1,000 sticks + 40% GST) raised prices, with a wider brand spread than one figure captures — Gold Flake ≈ ₹12/stick, Classic Milds ≈ ₹24/stick, pack ≈ ₹120–240. Store as per-stick × count, not a flat tier. | | |
| Sugar | usually ₹0–50 — logged for score, mostly ignored by budget | | |

*Costs are placeholder audits pending a full Phase 2 sweep. Partial audit done 2026-07-12 (`research/2026-07-12_bangalore-vice-cost-audit.md`): smoking updated above [verified]; alcohol premium row brackets real Toit/Byg Brewski menus ₹1,500–3,000+/person [verified] but the budget-tier row rests on cost-of-living estimates [hypothesis]; food-delivery AOV **could not be verified from primary sources** (Swiggy/Zomato investor PDFs are image-only) — the ₹425 figure is search-synthesized [hypothesis]. The ₹80 lunchbox baseline needs a scope decision — raw ingredients (₹25–40/portion [verified]) vs fully-loaded (LPG/protein/waste) — before it drives the savings ledger.*

---

## DOMAIN 1: THE CORE BALANCE ENGINE (Math — refactored & verified v0.2)

*Revision note: v0.1's EMA had two structural flaws found on re-derivation: (a) the day-score ceiling was 70, not 100 — a perfect user could never score above 70; (b) sleep had silently dropped out of the model entirely. Both fixed below. Every constant remains a calibration placeholder.*

### 1. Day Quality Score ($D_t$) — what today earned

Four earnable components sum to exactly 100 on a perfect day:

$$D_t = \mathrm{clamp}_{[0,100]}\Big(\,\underbrace{35}_{B}\; +\; \underbrace{Z_t}_{\text{sleep} \le 25}\; +\; \underbrace{S_t}_{\text{movement} \le 25}\; +\; \underbrace{15\,N_t}_{\text{nutrition}}\; -\; \underbrace{m_t\, V_t\,(1 - 0.5\,N_t)}_{\text{vice penalty}}\Big)$$

| Component | Formula | Max | Notes |
|---|---|---|---|
| **$B$ — baseline** | fixed 35 | 35 | Credit for showing up & logging at all |
| **$Z_t$ — sleep credit** | $25 \cdot \min(1,\, h_t/8)$ | 25 | $h_t$ = sleep hours (self-report or wearable) |
| **$S_t$ — movement credit** | $\min(25,\, 0.5\, a_t)$ | 25 | $a_t$ = active minutes; 50 min maxes it (cap stops a 3-hour session dwarfing everything) |
| **$N_t$ — nutrition gate** | binary, below | +15 | Also halves the vice penalty when met |
| **$m_t$ — sleep vice-amplifier** | $\mathrm{clamp}_{[1.0,\,1.8]}\big(1 + 0.1(8 - h_t)\big)$ | ×1.8 | Continuous version of v0.0's stepped multiplier (steps create cliff effects: 5.9h vs 6.1h shouldn't differ hugely) |
| **$V_t$ — adjusted vice load** | below | capped 100 | |

**Check:** perfect day = 35 + 25 + 25 + 15 − 0 = **100** ✓. Sleep hits you twice on a bad night — less credit ($Z$) *and* amplified vice damage ($m$) — matching the verified physiology (alcohol wrecks sleep, and poor sleep slows vice recovery).

### 2. Nutrition Gate ($N_t$) — binary, single-defined

$$N_t = \begin{cases} 1, & \text{if } P_{in} \ge 1.2M \text{ grams AND } |E_{in} - E_{out}| \le 0.15\,E_{out} \\ 0, & \text{otherwise} \end{cases}$$

$M$ = body mass (kg) → protein floor 1.2 g/kg (72 kg → ~86 g). $E_{out}$ = estimated TDEE; the gateway is ±15%. When met: **+15 pts and the day's vice penalty is halved** — the "Protein Shield" story ("eating right blunts the night out") in one boolean. Yes, that's a deliberate double reward: it's the single behavior the app most wants to buy.

### 3. Adjusted Vice Load ($V_t$) — self-report first, physiology tops up

$$V_t = \min\Big(100,\; V_{sub,t} + \Delta_t\Big)$$

- $V_{sub,t}$ = **sum of all vice severities logged today** (stack additively: `Blackout` 50 + `Pack` 40 = 90 — the EMA smoothing, not per-vice clamps, provides the forgiveness)
- $\Delta_t$ = wearable-detected *unaccounted* strain — **can only add, never subtract or replace** what the user said:

$$\Delta_t = \min\Big(25,\; \omega\big(\hat r_t + \hat h_t\big)\Big), \qquad \hat r_t = \max\!\Big(0, \tfrac{RHR_t - RHR_{base}}{RHR_{base}}\Big), \quad \hat h_t = \max\!\Big(0, \tfrac{HRV_{base} - HRV_t}{HRV_{base}}\Big)$$

- Each ratio floors at 0 separately — a *good* RHR can't cancel out a *bad* HRV.
- **$\omega = 20$, anchored to real data:** a typical alcohol night (Oura 600k [verified]: lowest RHR +8.2%, HRV −15.6%) gives $\Delta = 20 \times 0.238 \approx 4.8$ — physiology quietly adds ~half a `Buzzed` tier when the body shows standard drinking strain; an extreme night (+20% RHR, −35% HRV) adds ~11; hard cap 25 so a sensor glitch can never nuke a score.
- No wearable → $\Delta_t = 0$, pure self-report. **The core works wearable-free [required for India].**
- UI never surfaces $\Delta$ as an accusation — the score absorbs it silently; the morning message stays supportive.

### 4. The Life Score ($LS_t$) — the rolling portfolio

$$LS_t = \alpha\, LS_{t-1} + (1-\alpha)\, D_t, \qquad \alpha = 1 - \tfrac{2}{N+1}$$

| Window | $\alpha$ |
|---|---|
| 7-day (default) | 0.75 |
| 30-day (long view) | ~0.94 |

Seed $LS_0 = 60$.

**Provable properties** (these are the product philosophy, in math):

1. **Bounded for free:** since $D_t \in [0,100]$ and $LS_0 \in [0,100]$, $LS_t \in [0,100]$ forever by induction — no output clamp needed.
2. **The Forgiveness Guarantee:** worst possible day ($D_t = 0$) costs $(1-\alpha)\,LS_{t-1}$ — **at most 25% of your current score** (α=0.75). No single night can crater you.
3. **The Bounce:** a good recovery day ($D_t = 90$) recovers ~10 points/day from a dip — crash −17, back in ~2 days. The name is the mechanic.

**Missing-data policy:** unlogged days **freeze** the score (no EMA step, no decay). Decay would be a streak mechanic wearing a costume — against the whole philosophy. After 3+ silent days: gentle re-entry message, zero penalty.

### 5. Worked Examples (arithmetic hand-verified)

**Day 1 — a drink on an otherwise great day.** $LS_{t-1}=65$. Sleep 7h ($Z=21.9$, $m=1.1$), 45-min lift ($S=22.5$), nutrition met ($N=1$), one `Philosophical` (25), no wearable:
$$D = 35 + 21.9 + 22.5 + 15 - 1.1 \times 25 \times 0.5 = 94.4 - 13.8 = 80.6$$
$$LS = 0.75(65) + 0.25(80.6) = \mathbf{68.9} \;(\uparrow 3.9)$$
*The score went UP on a drinking day — deliberately. A vice absorbed by an otherwise strong day shouldn't hurt: that's the portfolio thesis. (Without the drink it would've been 72.4 — the drink cost 3.5 net.)*

**Day 2 — the blowout.** $LS_{t-1}=68.9$. Sleep 4.5h ($Z=14.1$, $m=1.35$), no workout, gate blown ($N=0$). Logged: `Blackout` 50 + `Drunk Cigs` 18 + `Late-Night Binge` 15 → $V_{sub}=83$. Wearable: RHR +20%, HRV −35% → $\Delta = 20(0.20+0.35)=11.0$ → $V=94.0$ *(errata 2026-07-12: HRV ratio pinned to the clean $\hat h = 0.35$ per PRD-01 Q5; final $D$/$LS$ unaffected)*:
$$D = 35 + 14.1 + 0 + 0 - 1.35 \times 94.1 = -78 \;\rightarrow\; \mathbf{0}$$
$$LS = 0.75(68.9) = \mathbf{51.7} \;(\downarrow 17.2 = \text{exactly the 25\% bound})$$

**Day 3 — the bounce.** $LS_{t-1}=51.7$. Sleep 8h ($Z=25$, $m=1.0$), 30-min walk ($S=15$), ate clean ($N=1$), zero vices:
$$D = 35 + 25 + 15 + 15 = 90, \qquad LS = 0.75(51.7)+0.25(90) = \mathbf{61.3} \;(\uparrow 9.6)$$

Crash bounded at −17, recovery +10/day, back near baseline in two days. The user *sees* the path back instead of a broken streak.

### 6. Financial Recalibration (the second arrow of the loop)

Onboarding sets a **weekly discretionary vice budget** $W$ (defaults by wallet tier: ₹1,500 / ₹3,000 / ₹6,000 — placeholder, Phase 2 audit). Every vice log carries its cost $c_i$ (tier default, editable). On each log:

$$R = W - \sum_{\text{this week}} c_i \qquad\qquad a_{daily} = \frac{\max(0, R)}{\text{days left in week}}$$

**Worked example:** $W$=₹3,000, ₹800 already spent. Friday's `Philosophical` night (₹₹ default ₹1,200) + a delivery order (₹400) → $R = 3{,}000 - 800 - 1{,}600 = ₹600$, 3 days left → **"₹200/day of fun money till Monday — cook Saturday and Sunday and you're clear."**

**No-rollover rule:** overshooting ($R<0$) shows the overshoot once, then **resets clean on Monday**. Debt-carrying is a guilt mechanic in a spreadsheet costume — same reason the score freezes instead of decaying. This is the financial "never miss twice."

**Leftover / Lunchbox Loop (savings side):** at dinner logging, prompt double-portion cooking; confirmed lunchbox next day credits `(lunch delivery baseline − home cost)` to a visible savings ledger — **~₹150/day at every wallet tier** (founder-set 2026-07-12: baselines ₹250/350/450 − home costs ₹100/200/300, both `[hypothesis]`, per PRD-02 FR6; supersedes the earlier ₹120–300 range). Weekly savings injection framed in visceral terms ("most of a tank of petrol").

### 7. Recovery Mode (the third arrow) — unified trigger

Enter for up to **48h** (an optimistic-median rebaseline window — see the Rebaseline row in the Research Digest; [hypothesis], not a design guarantee, so the 48h is a tunable placeholder) when **any** of:
- $V_t \ge 25$ (one Philosophical night or equivalent stack), or
- $h_t < 5$ hours, or
- wearable recovery < 50% (when connected)

Effects: strain/workout targets ×0.75 (−25%, mid of the 20–30% evidence-backed range), hydration +1L, dark low-stimulation theme, workout rings hidden, single walk+water focus. **Early exit** if next day: $h \ge 7$ and zero vices. Manual override always available.

---

## DOMAIN 2: ADAPTIVE UNIFIED INTERFACE (UX & Frontend)

*Largely unchanged from v0.1; trigger logic now references the unified Recovery Mode above.*

- **One-Shot Dashboard:** single canvas, animated Life Score Ring, today's budget line under it (the loop's two numbers, one glance).
- **Two-tap vice logging:** domain → tier. Cost pre-filled from wallet tier, tap-to-edit. Target: < 3 seconds.
- **Dual-Track Toggle `[Advanced Metrics]`:** casual users see score + budget only; strict users unfold macros, $D_t$ components, RHR/HRV trends. Same backend ledger either way.
- **Swipe gestures:** right-swipe logs check-ins; double-tap opens vice modal; long-press = history.
- **Recovery Mode UI:** dark soothing theme, suppressed exercise metrics, hydration tracker front-and-center, circadian-reset prompt.

## DOMAIN 3: INTELLIGENT NUDGE NETWORK (Behavioral)

*Unchanged mechanics; delivery risk moved to Part 2 (the funnel metric).*

- **08:30 Commute Nudge:** hydration/circadian, silent, supportive.
- **11:15 Pre-Lunch Pivot:** wallet-tier + dietary-preference matched lunch recommendation; in Recovery Mode, recovery-optimized options first.
- **20:30 Dinner & Prep Loop:** log dinner → double-portion prompt → lunchbox savings credit.
- **Nudge copy discipline:** every message shows *the path back*, never the deficit alone. "Nice logging" > "you exceeded."
- **Crowdsourced Global Kitchen:** user recipes → automated macro sanity check (kcal-vs-macro variance ≤10%, protein density ≤40% of weight) → community upvotes → published. Seed with 20 founder recipes.
- **Cap: 2 nudges/day default** — nudge fatigue is a named risk; frequency is a pilot variable.

## DOMAIN 4: WHAT'S DELIBERATELY OUT (and why) — the affordability fence

| Feature | Status | Reason **[verified]** |
|---|---|---|
| Bank data via Account Aggregator | **Out — permanently for this stage** | FIU must be RBI/SEBI-regulated; no TSP bypass; own license needs ₹2 Cr NOF. A funded-company problem. |
| Gmail transaction parsing | **Out** | $15–75k Google CASA assessment for restricted scopes |
| SMS transaction parsing | **v2 candidate only** | Play-policy-viable only as disclosed core function; documented rejections; Android-only. Revisit after manual logging proves retention. |
| Whoop/Oura-first integration | **Demoted to premium optional** | India niche: smart rings −30.6% (2025), Oura India ₹29k+. Health Connect (Android 95%) + Ultrahuman API are the India-relevant candidates — post-MVP. |
| Public leaderboards | **Out permanently** | Stigma amplifier in India; private pods only, later. |
| Native iOS | **Deferred** | $99/yr + small share; PWA degrades gracefully there. |

---

---

# PART 2: THE PLAN — Validation Protocol v0.4 (build-first)

*v0.4 restructure — founder decision 2026-07-12: the WhatsApp concierge pilot is dropped. Build-first: the live cohort is now the first and only real evidence gate. The moat question moves in-app. Trade-off owned explicitly: this converts a ₹0, 2-week falsification of the riskiest assumption into a ~3-week build testing it with a blunter metric — accepted because the pilot's friends-cohort evidence was itself confounded. Total: ~7 weeks of evenings. Every phase keeps a kill switch.*

## Phase 0 — Foundations (Week 0)

- Finalize vice taxonomy + tier costs (the tables above) as the cohort's shared vocabulary
- **DPDP basics:** privacy policy, named grievance officer + email, consent-log design — a few evenings of template work, done *before* collecting anyone's data
- Recruit cohort: 30–40 profile-matched users (15 tech workers HSR/Indiranagar, 15 students), **≥12 of whom commit to the Play closed test** — now **load-bearing**: they are the Week-3 checkpoint cohort, not just Google's 12-tester requirement
- Define instrumentation up front: the **funnel events** (link click → home-screen add → notification permission) and retention events
- **Recruitment-embedded copy probe [new in v0.4]:** during recruitment conversations (happening anyway), show the mock reflow message ("₹200/day of fun money till Monday…") and record verbatim reactions. Not a gate, not a pilot — instrumented recruitment at ₹0. This is the cheapest surviving pre-code signal on the moat.

## Phase 1 — Build Sprint (Weeks 1–3)

**Stack:** Next.js + Tailwind on Vercel, Supabase (free tiers), TypeScript, localStorage offline-first — unchanged. **Key mechanics [verified rationale]:**

- **TWA track from Week 1:** Bubblewrap-wrap the PWA (~a day of work), pay the $25 one-time Play fee, start the 14-day closed test with the committed 12 — the testing clock closes inside the sprint
- **Funnel instrumentation before features:** if we can't measure link→install→permission, we can't interpret anything else

| | Founder A (engine) | Founder B (frontend) | Founder C (platform) |
|---|---|---|---|
| Wk 1 | $D_t$/EMA engine + budget reflow, unit tests against the worked examples above (PRD-01/02) | Ring + budget line canvas, two-tap log flow | Supabase auth/schema, funnel events, consent log, TWA wrap + closed test start |
| Wk 2 | Recovery Mode logic, savings ledger | Swipe gestures, Recovery theme | Push notifications |
| Wk 3 | Buffer + bugfix | Responsive pass | DPDP checklist, deploy |

**Parallel desk track (absorbs old Phase 2, descoped):** Bangalore Top-100 foods JSON (household units → macros) + the remaining cost-audit gaps (delivery baseline, OQ1 in PRD-02). **Nudge copy v0** is founder-written `[hypothesis]`, sourced from the recruitment copy-probe verbatims + the taxonomy's own culturally-worded tiers; copy is an explicit iterate variable at the Week-6 gate. *Capacity note: this was a serial week in v0.3 — the side-project honesty rule (2-week slip → re-scope) applies with extra force to Weeks 1–3.*

**MVP fence:** manual logging only — no wearables, no SMS, no recipes directory (seed content only). The worked examples in Domain 1 (now pinned in PRD-01/02) are the acceptance tests.

## Week-3 Checkpoint — closed-test cohort (n=12) · *fix-before-spend, not a product verdict*

Scoped to what n=12 can measure — funnel and activation, **not** retention. Bands `[hypothesis — no citable benchmark for warm committed-tester conversion; logic: if people who promised to install can't get through, cold users have no chance]`:

| Metric | Proceed | Fix first | Halt onboarding |
|---|---|---|---|
| Warm install funnel (link→installed→notif), committed testers | ≥ 9/12 | 6–8/12 | < 6/12 |
| Activation (first vice log ≤48h of install) | ≥ 8/12 | 5–7/12 | < 5/12 |
| Golden fixtures pass (a_daily==200; crash-free) | pass | — | fail |

This moves funnel-leak discovery (the register's top risk) from Week 7 to Week 3.

## Phase 2 — Live Cohort (Weeks 4–6) · *the decision gate*

Onboard the full cohort to the real app (PWA link + Play closed-test track). Extended 2→3 weeks: D7 needs onboarding spread plus a D14 confirmatory read now that nothing precedes this gate. Two arms:

- **Warm arm:** the recruited 30–40.
- **Cold arm [new in v0.4]:** 10–15 users with no founder tie (college group / Bangalore subreddit) — replaces the dead pilot as the inflation-calibration instrument.
- **Week-5 structured interviews (n≥6)** using the retired concierge script's language as the interview guide — recovers the pilot's lost qualitative budget-reflow signal.

**Gates — retention/funnel bands benchmark-anchored [verified: category median D7 ≈ 7–20%, good = 28–40%]; loop-engagement split in v0.4:**

| Metric | Exceptional | Strong (continue) | Iterate | Kill |
|---|---|---|---|---|
| **D7 retention** (warm) | ≥ 40% | ≥ 30% | 20–30% | **< 20%** |
| **Install funnel** (click→installed→notif granted) | ≥ 60% | ≥ 40% | 25–40% | < 25% |
| **Unprompted budget-surface opens** — *the moat test*: % of actives opening the budget surface ≥2×/wk NOT within 15 min of a log | ≥ 60% | ≥ 40% | 20–40% | < 20% |
| Avg session | ≥ 90s | ≥ 60s | 30–60s | < 30s |

- *Reflow delivery-view* (old "loop engagement") is demoted to a **hygiene metric**: expected ≥80%; <60% = delivery/instrumentation bug, not a product verdict `[hypothesis]`. The pushed message is near-automatic to see — as a gate, nobody could fail it.
- Moat-test bands `[hypothesis — no citable category benchmark for feature-level unprompted revisit; same bands as v0.3's loop engagement on a deliberately stricter metric — appropriate for the only gate left]`. Secondary confirmations: cost-edit rate (correcting the ₹ = caring about the ₹), reflow-screenshot shares.
- **Cold-arm qualifier `[hypothesis]`:** warm-cohort "Strong" counts only if cold-arm D7 is within ~10–15 pts of warm; a bigger gap = inflation → treat as Iterate.
- *v0.1's D7 ≥45% gate remains retired as pass/fail; it survives as the "Exceptional" column.*

**Decision framework at Week 6:**
- **Strong+ on retention AND funnel** (cold-arm qualified) → open beta, Play production listing, start v2 list (SMS parsing, Health Connect)
- **Iterate band** → one focused 2-week fix cycle on the single worst metric, re-gate once
- **Kill band on retention** with healthy funnel → the product isn't wanted; write the postmortem, keep the learnings
- **Kill band on funnel** with decent in-app retention → distribution problem, not product problem; push the Play-Store track harder before judging the product

## Risk Register (v0.4)

| Risk | Likelihood | Mitigation |
|---|---|---|
| Install/permission funnel leaks silently | **High — the top risk** | Instrument first; Week-3 checkpoint catches it 4 weeks earlier than v0.3; Play-Store track as trust fallback; in-chat install walkthrough |
| Budget reflow feels like surveillance, not care | **High — raised in v0.4** (its old mitigation, the Phase-1 qualitative gate, died with the pilot) | Recruitment copy probe (Wk 0) + Week-5 interviews + unprompted-opens metric |
| **Sunk-cost pressure at the only gate [new]** | High — 3 weeks of build before any evidence; founders will want to soften a Week-6 kill | All three founders sign the v0.4 bands **before Week 1** |
| Warm-cohort inflation (no manual-mode baseline anymore) | High | Cold arm inside the live cohort; cold-arm qualifier on the gate |
| **Untested nudge copy [new]** | Medium — copy v0 is founder intuition; compounds nudge fatigue | Copy is an explicit iterate variable at the gate |
| Nudge fatigue | Medium | 2/day cap, frequency as a cohort variable |
| Severity weights feel unfair/arbitrary | Medium (slightly worse — engine ships with zero human-in-loop calibration) | Ship as "beta weights," collect disagreement in-app, retune with cohort data |
| A stealth competitor occupies the loop | Low — **sweep done 2026-07-12, clear** | Re-sweep before open beta |

---

---

# PART 3: ENGINEERING NOTES — Illustrative Sketch (updated)

*Still one plausible build, not a commitment. Deltas from v0.1 only; unchanged boilerplate (CI/CD, repo layout, API route sketches) trimmed — regenerate when actually building.*

### Stack (₹ ~2,000 one-time, ~₹0/month)

| Layer | Choice | Change from v0.1 |
|---|---|---|
| Frontend/Hosting | Next.js + Tailwind on Vercel free | — |
| Backend | Supabase free (Postgres + Auth + Edge Functions) | — |
| **Android store** | **TWA via Bubblewrap + $25 Play fee** | **New — pilot cohort satisfies 12-tester rule** |
| Wearables | **None in MVP.** v2: Health Connect (Android majority) + Ultrahuman API; Whoop/Oura = premium optional | Was Whoop-first — retired for India |
| Finance data | **Manual logging only.** v2 candidate: SMS parsing (disclosed-core, Android) | Was AA-curious — verified out of reach |
| Notifications | Web Push (Chrome) / FCM via TWA | — |

### Schema deltas (v0.1 → v0.2)

**`vices_logged` rows** gain: `cost_inr` (int, tier default, user-editable), `severity` (positive int — never negative, see Domain 1 sign-convention note).

**New `weekly_budgets`:** `{ user_id, week_start, budget_inr, spent_inr, computed daily_allowance }` — no rollover column *by design*.

**New `funnel_events`:** `{ anon_id, event: link_click | pwa_install | notif_granted | first_log, ts, source }` — written from day one, pre-auth (anonymous id), because the funnel starts before signup.

**`calculated_scores`** (per day): `{ Z, S, N, m, v_sub, delta_physio, v_adj, D, alpha, LS_prev, LS }` — store every component, not just the result, so weight retuning can be backtested against history.

**DPDP additions:** `consent_log` table (what was consented to, when, version); grievance-officer contact in app + policy; data-deletion endpoint stub.

---

---

# PART 4: TEAM CHARTER & KPI DASHBOARD — Draft Working Agreement

### Ownership (by domain, unchanged structure)

| Founder | Owns | AI workflow |
|---|---|---|
| **A (PM/Lead)** | Balance Engine math, budget reflow, PRD | Claude Code — logic, schema, unit tests vs worked examples |
| **B (Frontend)** | Ring canvas, gestures, Recovery theme | UI agents, Framer Motion |
| **C (Platform)** | Supabase, push, TWA/Play, funnel events, DPDP | Edge functions, auth, store pipeline |

Cadence: daily 15-min standup, Friday demo+retro. Side-project honesty rule: if two consecutive weeks slip, re-scope the phase rather than silently extending — the plan only works if the gates stay dated.

### KPI Dashboard (gates live in Part 2; this is the watch-list)

| Tier | Metric | Watch level |
|---|---|---|
| **North star** | D7 retention (warm, cold-arm qualified) | ≥ 30% |
| **The silent killer** | Link→install→notif-permission funnel | ≥ 40% |
| **The moat test** | **Unprompted budget-surface opens** (≥2×/wk, not within 15 min of a log) | ≥ 40% |
| Hygiene | Reflow delivery-view after vice log | ≥ 80% (below 60% = instrumentation bug) |
| Behavioral | Vice confessions ≥2/wk per active user | ≥ 40% of actives |
| Behavioral | Recovery-Mode hydration/walk action rate | ≥ 60% |
| Economic | Confirmed lunchbox saves/user/week | ≥ 2 |
| Retired | ~~Whoop adoption ≥75%~~ | wearable attach is informational only |
| Retired | ~~D7 ≥45% as pass/fail~~ | now the "Exceptional" band |

---

# APPENDIX

### Glossary (v0.2)

| Term | Definition |
|---|---|
| **The Loop** | One vice log → Life Score absorption + budget reflow + 48h recovery adjustment. The verified whitespace; the product. |
| **$D_t$ — Day Quality** | 0–100: baseline 35 + sleep ≤25 + movement ≤25 + nutrition 15 − amplified vice penalty |
| **$LS_t$ — Life Score** | EMA of day quality; α=0.75 (7-day). Bounded [0,100] by construction. |
| **Forgiveness Guarantee** | Provable: no single day can cost more than 25% of the current score |
| **$N_t$ gate** | Binary: protein ≥1.2 g/kg AND calories within ±15% TDEE → +15 pts and vice penalty halved |
| **$\Delta_{physio}$** | Wearable strain top-up; ω=20 (Oura-anchored); add-only, capped 25; zero without wearable |
| **$m_t$** | Sleep vice-amplifier, 1.0×@8h → 1.8×@0h, continuous |
| **Budget reflow** | Weekly discretionary pot minus logged vice costs, re-divided over remaining days; no rollover |
| **Vice tier** | Qualitative severity label, positive magnitude, culturally-worded (`Sutta-Chai Break`) |
| **Recovery Mode** | ≤48h reduced-target soft-UI state (48h is an optimistic-median rebaseline placeholder, [hypothesis] — see Research Digest); triggers: $V_t\ge25$ ∨ sleep<5h ∨ wearable recovery<50% |
| **TWA** | Trusted Web Activity — a thin Android wrapper that ships the existing PWA through the Play Store; ~free via Bubblewrap |

### What would change our minds (v0.4, build-first)

- **Kill signals:** D7 < 20% warm; unprompted budget-opens < 20% with healthy delivery-views (the moat isn't wanted); funnel < 25% after Week-3 fixes; interview majority describing the reflow as "being watched"
- **Double-down signals:** unprompted budget-opens ≥ 40% + meaningful cost-edit rate; D7 ≥ 30% with cold arm within ~10 pts; users referencing the ₹ number unprompted in Week-5 interviews; organic reflow-screenshot shares
- **Re-scope signals:** score engages but budget surface ignored (→ health-only fallback), or vice-versa (→ PFM-lite pivot); funnel kill-band with decent in-app retention (→ distribution fix before product verdict)

### Version History

| Version | Date | Changes |
|---|---|---|
| 0.1 | Jul 2026 | First idea capture; hypothesis framing; EMA math v1 |
| **0.2** | **Jul 2026** | **Merged concept (health+finance loop = core, [verified] whitespace); math refactored — 100-pt day-quality fixed (v1 capped at 70), sleep restored as credit + amplifier, ω Oura-anchored, provable bounds + worked examples; vice set broadened (delivery, sugar) with cost defaults; retention gates recalibrated to category benchmarks (kill <20%, not <45%); PWA+TWA dual-track distribution; funnel promoted to primary metric; AA/Gmail verified out of reach — manual-first locked; DPDP checklist added; Whoop-first retired for India** |
| **0.3** | **Jul 2026** | **Claim-hardening pass (research + claim-verifier, 2026-07-12): "24–48h rebaseline" un-attributed from Oura (that study has no recovery-timeframe data) and re-sourced as [hypothesis — MunichBREW II, Eur Heart J 2024] — ~24h under heavy dosing, but WHOOP real-world shows a 4–5-day tail; the 48h Recovery window is now explicitly a tunable placeholder, not a guarantee. RHR figure corrected to *lowest* RHR +8.2% (source's own label), not average. Closed-loop moat sweep completed (App/Play + Crunchbase/Tracxn) — whitespace survives, sharpened to *prospective re-plan* vs retrospective dashboards; Paceline / Aditya Birla cited as adjacent-but-opposite validation. Smoking costs updated for the Feb 2026 excise overhaul [verified]; alcohol/food/lunchbox costs partially audited with open gaps flagged. Sources: `research/2026-07-12_*.md`.** |
| **0.4** | **Jul 2026** | **Build-first restructure (founder decision 2026-07-12, gate redesign by ceo-strategist): WhatsApp concierge pilot dropped. New sequence: Phase 0 (wk 0, + ₹0 recruitment-embedded copy probe) → build sprint (wks 1–3, old Phase-2 data work folded in as a parallel desk track) → Week-3 closed-test checkpoint (n=12, funnel + activation, fix-before-spend) → live cohort (wks 4–6, warm + cold arms, Week-5 interviews) as the first and only decision gate. Loop-engagement metric split: reflow delivery-view demoted to hygiene (≥80%); the moat test is now *unprompted budget-surface opens* [hypothesis bands]. Cold-arm qualifier on "Strong." Risk register: surveillance risk raised to High (its Phase-1 mitigation died), new sunk-cost-at-the-only-gate and untested-copy risks; competitor risk lowered (sweep clear). New riskiest assumption: users want the prospective ₹ number at all — first evidence now arrives Week 4+ in-app instead of Week 2 in chat.** |
