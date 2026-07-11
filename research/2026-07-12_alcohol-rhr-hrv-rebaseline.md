# Research Digest: Alcohol → RHR/HRV Rebaseline Window ("24–48h" claim)

**Date:** 2026-07-12
**Assigned by:** ceo-strategist (follow-up to claim-verifier finding that the venture doc's "24–48h to rebaseline" figure is not in the cited Oura 600k-member source)
**Scope:** Find a primary source (peer-reviewed physiology/sleep literature or well-documented wearable study) that supports a specific timeframe for resting heart rate (RHR) / HRV to return to baseline after acute alcohol consumption. Does NOT cover hangover symptom scales, cognitive/mood recovery, or non-cardiac markers — out of scope, noted below.

## Table

| Finding | Verdict | Consequence for Bounce |
|---|---|---|
| The cited Oura 600k-member analysis reports same-night HR/HRV deltas only (avg HR +9.6%, lowest RHR +8.2%, HRV −15.6%/−10.8ms) and gives **no recovery timeframe** at all — confirms the prior claim-verifier finding. | Confirmed (absence) | The "24–48h" figure cannot be attributed to this source under any interpretation; it must cite something else or drop to `[hypothesis]`. |
| No single peer-reviewed acute-dosing study we found measures RHR/HRV long enough (i.e., past ~10 hours) to empirically pin down a "return to baseline" time. | Unverified | Bounce cannot claim a precise, source-backed hour count for full autonomic rebaseline from acute literature alone. |
| The IV-ethanol RCT (Brunner et al. 2021, *Sci Rep*) found HRV markers (SDNN, RMSSD, LF, HF) were **still below baseline** even after breath alcohol had cleared to near-zero (~10.4h post-infusion) — i.e., incomplete recovery persisted beyond the point of "sobering up." | Confirmed (for its own protocol; n=15) | Suggests a 24–48h window is plausible as a lower bound, not an overshoot — the "no rebaseline before ~10h" data point is consistent with a longer (24h+) full-recovery window, though it doesn't prove one. |
| The Finnish real-world sleep study (Pietilä et al. 2018, *JMIR Mental Health*, n=4,098) only tracked the **first 3 hours of sleep**; low-dose (≤0.25 g/kg) HR/recovery approached reference levels by hour 3 (visual inspection only, not statistically modeled beyond that), high dose (>0.75 g/kg) did not. | Unverified beyond 3h | Cannot be used to support or refute 24–48h; the study simply doesn't run long enough. |
| The only source with a multi-day recovery curve is WHOOP's own "Four-Day Hangover" analysis (n=148 collegiate athletes, single 2015–16 season, self-reported single-item drinking flag, non-peer-reviewed company blog): 74% suppressed recovery day 1, 29% day 2, 19% day 3, "some" day 4–5. | Likely (as WHOOP's own data) but methodologically weak | This is the closest thing to an actual timeline and it does **not** cleanly support "24–48h to rebaseline" — a fifth to a third of the cohort is still below baseline past 48h, with a tail to 96–120h. A precise "24–48h" cutoff rounds this down. |
| A 9-day smartwatch protocol (n=40, *Nutrients* 2025, PMC12073130) of *repeated* moderate drinking (3 consecutive nights, not a single acute episode) found RHR "returned rapidly to near-baseline" in the 3-day post-exposure phase, but only day-level granularity — no hour-level data, and the exposure pattern (multi-night, not single-night) doesn't match Bounce's single-log use case. | Unverified (wrong protocol for the claim) | Cannot be used to source a single-night 24–48h claim; different denominator (chronic short-term exposure vs. one night out). |
| Acute lab dosing studies (Spaak et al. 2010, *AJP-Heart Circ Physiol*, n=12; Pietilä 2018) establish that alcohol suppresses HRV/raises HR acutely and dose-dependently, but none of them report a recovery/rebaseline time point at all. | Confirmed (for acute effect only) | Good evidence that vagal suppression happens and scales with dose — no evidence for how long it lasts. |

## Detail per finding

### 1. Oura 600k-member blog (the originally-cited source)
- **Source:** ["Oura Data Reveals the True Impact of Alcohol on Sleep"](https://ouraring.com/blog/how-does-alcohol-impact-oura-members/), Oura Ring "The Pulse" blog.
- **What it says:** De-identified aggregate data from >600,000 Oura members, Jan–Oct 2025, comparing nights tagged "alcohol" to surrounding alcohol-free nights. Average HR +9.6%, lowest RHR +8.2%, HRV down ~15.6% (mean −10.8ms), sleep score down 6.8%, sleep duration −35 min.
- **On recovery timeframe:** `[verified — Oura "How Does Alcohol Impact Oura Members" blog]` — **the article contains zero recovery-duration data.** It compares only the drinking night to adjacent nights; it never states how many hours/days it takes to return to baseline. This directly confirms the earlier claim-verifier finding: the 24–48h figure is not derivable from this source under any reading.

### 2. Brunner et al. 2021, "Impact of acute ethanol intake on cardiac autonomic regulation," *Scientific Reports* 11:13255.
- URL: https://www.nature.com/articles/s41598-021-92767-y (also PMC: https://pmc.ncbi.nlm.nih.gov/articles/PMC8225621/)
- n=15 healthy volunteers (8M/7F, mean age 28.8±6.8), IV ethanol targeted to breath alcohol concentration (BrAC) 0.50 mg/l, ECG at baseline, peak BrAC, and after BrAC cleared to ~0.05 mg/l (mean elapsed time ≈10.4±1.3 hours).
- `[verified — Brunner et al. 2021, Sci Rep]`: HR rose from 66.5±6.1 to 76.0±9.4 bpm at peak alcohol (p<0.001) and "remained elevated" after clearance. Standard HRV (SDNN, RMSSD, LF, HF) "slightly increased after alcohol levels returned towards normal without reaching baseline levels" — i.e., HRV was *still suppressed* ~10 hours after drinking, once alcohol itself had nearly cleared. Deceleration capacity (an autonomic marker) did return to baseline by that point; periodic repolarization dynamics did not (p=0.042 vs. baseline).
- **Implication:** this is evidence that autonomic recovery lags behind alcohol clearance — consistent with (but not proof of) a >10h, plausibly 24h+ window — but the study wasn't run long enough to say when HRV *does* normalize.

### 3. Pietilä et al. 2018, "Acute Effect of Alcohol Intake on Cardiovascular Autonomic Regulation During the First Hours of Sleep," *JMIR Mental Health* 5(1):e23. DOI: 10.2196/mental.9519.
- URL: https://mental.jmir.org/2018/1/e23/ (PMC: https://pmc.ncbi.nlm.nih.gov/articles/PMC5878366/)
- n=4,098 Finnish employees (55.8% female, mean age 45.1), real-world within-subject design, beat-to-beat R-R recordings, first 3 hours of sleep only.
- `[verified — Pietilä et al. 2018, JMIR Mental Health]`: dose-dependent effect — low dose (≤0.25 g/kg) raised HR +1.4 bpm and cut "recovery %" by 9.3 points; high dose (>0.75 g/kg) raised HR +8.7 bpm and cut recovery % by 39.2 points, measured across the first 3 hours of sleep. Authors note (their words, per fetch): low-dose curves approach reference levels by hour 3 on visual inspection; no quantitative claim is made and no data exists past hour 3.
- **Implication:** cannot confirm or deny 24–48h — the observation window ends at 3 hours.

### 4. WHOOP "The Four-Day Hangover" (company blog, own aggregate data)
- URL: https://www.whoop.com/us/en/thelocker/the-four-day-hangover-hrv-alcohol/ (WebFetch blocked this URL with HTTP 403; content reconstructed via WebSearch snippets of the same page and independent secondary summaries — flagged below as a limitation).
- n=148 student-athletes across 11 teams/6 sports, 2015–2016 season, self-reported single-item "drank last night" flag in the WHOOP journal (not dose-controlled, not blinded, not peer-reviewed).
- `[hypothesis]` (secondary-sourced quotes of a primary-owner's own blog, not independently fetched in this session): RHR +16.2%, HRV −22.7% the night after drinking vs. non-drinking teammates ("equivalent to aging 12 years"). Recovery-metric suppression by day: 74% still below baseline day 1, 29% day 2, 19% day 3, "some" day 4–5.
- **Implication for the 24–48h claim:** this is the *only* source in this search with a genuine multi-day curve, and it argues against, not for, a clean 24–48h full-rebaseline claim — roughly a fifth to a third of the sample is still measurably suppressed beyond the 48h mark, with a real tail to 96–120h.

### 5. PMC12073130 / Nutrients 2025, "The Impact of Alcohol on Sleep Physiology: A Prospective Observational Study on Nocturnal Resting Heart Rate Using Smartwatch Technology."
- URL: https://pmc.ncbi.nlm.nih.gov/articles/PMC12073130/ (also https://www.mdpi.com/2072-6643/17/9/1470)
- n=40 healthy adults (63% female, mean age 30.5), Ludwig Maximilian University Munich. 9-day protocol: 3 baseline days, 3 consecutive drinking evenings (40g/day women, 60g/day men), 3 post-exposure days.
- `[verified — Nutrients 2025 / PMC12073130]`: nocturnal RHR rose from 63.6±9.2 to 66.6±9.0 bpm during the 3-night exposure block (p<0.001) and "returned rapidly to near-baseline" (64.9±9.3 bpm) in the 3-day post-exposure phase — but the paper reports only day-level means, not an hour-resolved recovery curve, and the exposure is repeated moderate drinking over 3 nights, not a single acute binge. Mismatched protocol for a single-log "vice" claim.

### 6. Spaak et al. 2010, "Dose-related effects of red wine and alcohol on heart rate variability," *AJP-Heart Circ Physiol*. DOI: 10.1152/ajpheart.00700.2009.
- URL: https://journals.physiology.org/doi/full/10.1152/ajpheart.00700.2009 (WebFetch returned HTTP 403; abstract/findings reconstructed via PubMed/ResearchGate/Semantic Scholar summaries — flagged as a limitation, not independently fetched in full).
- n=12 healthy subjects, 1 vs. 2 standard drinks (red wine or ethanol) vs. water, randomized single-blind, same-session measurement only.
- `[hypothesis]` (secondary-sourced, not independently fetched): 1 drink had no significant HR/HRV effect; 2 drinks cut total HRV 28–33% and HF power 32–42%. No follow-up beyond the acute session — no recovery data.

## What we could NOT verify

- **A precise "24–48h" figure for RHR/HRV rebaseline after a single acute drinking episode, from any primary source.** Searched: Oura's own blog (no timeframe given at all); PubMed/PMC/Google Scholar-adjacent searches for "alcohol HRV recovery time," "alcohol resting heart rate baseline hours," "acute ethanol HRV recovery duration." Found five studies with actual data (Brunner 2021, Pietilä 2018, Spaak 2010, PMC12073130/Nutrients 2025, WHOOP's own blog) — none states a 24–48h rebaseline window as a finding. What would settle it: a study that (a) measures a single acute alcohol episode (not repeated/chronic), (b) tracks continuous RHR/HRV for at least 72–96 hours post-episode, and (c) reports the time-to-baseline as a primary or secondary outcome (survival-curve style, e.g., "median time to return within X% of baseline = Y hours").
- **Full text of Spaak et al. 2010** — WebFetch was blocked (403) by journals.physiology.org; findings above are reconstructed from secondary summaries (PubMed abstract, ResearchGate, Semantic Scholar) and are tagged `[hypothesis]` accordingly, not `[verified]`, until the primary PDF/abstract is directly read.
- **Full text of WHOOP's "Four-Day Hangover" post** — WebFetch was blocked (403) by whoop.com; the day-by-day percentages above are reconstructed from WebSearch snippets that appear to quote the page directly and are corroborated across two independent search queries returning identical numbers, but this session did not independently fetch and confirm the primary page. Tagged `[hypothesis]` pending direct verification.

## Suggested next questions

1. Should the venture doc's Recovery Mode duration be re-derived from the WHOOP athlete curve (74%/29%/19% by day) instead of a single "24–48h" point estimate — e.g., a decaying/tapering Recovery Mode rather than a hard cutoff?
2. Is a "single acute episode, 72h+ continuous monitoring, time-to-baseline as an outcome" study findable at all in the literature, or does Bounce need to commission/estimate this itself (e.g., via a small pilot with Oura/WHOOP-wearing users)?
3. Should Bounce attempt to directly fetch the WHOOP blog and Spaak et al. full text (e.g., via a different access route) to upgrade those two citations from `[hypothesis]` to `[verified]`?
4. Given Bounce's target cohort (22–35 urban tech workers/students) skews younger and likely lighter-drinking than WHOOP's collegiate athletes or Pietilä's Finnish working-age sample — does the recovery window generalize, or does Bounce need cohort-matched data?

## Out of scope, noticed

- Hangover *symptom* scales (nausea, headache, concentration) resolve on a different, generally shorter timeline (~24h) per some literature (e.g., van Schrojenstein Lantman et al. work referenced in search results) — this is a distinct construct from cardiac autonomic (RHR/HRV) rebaseline and was not verified here; flagging in case Recovery Mode conflates "feels normal" with "is physiologically normal."
- Multiple wearable-company blogs (blēo, Kygo, Impossibrew) republish the WHOOP/Oura numbers with added color (e.g., "day 4 typically shows return to baseline for moderate drinkers") that do not trace to any primary source found in this search — these are blogs citing blogs and were excluded from the table entirely per the no-secondary-sourcing rule.
