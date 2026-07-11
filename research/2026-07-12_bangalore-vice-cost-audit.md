# Research Digest: Bangalore Vice-Cost Audit (Budget Reflow Feature)

**Date:** 2026-07-12
**Scope:** Validate/correct placeholder ₹ figures in the venture doc's budget-reflow
cost tables (alcohol, food delivery, cigarettes, home-cooked meal baseline) against
real 2026 Bangalore prices. Desk research only — no primary interviews.

**Placeholders under test** (from `docs/venture/Bounce_Strategic_Documentation_Suite.md`):
- Alcohol: T1/T2/T3 × budget/mid/premium = 300/600/1000, 600/1200/2500, 1200/2500/5000
- Food delivery: T1/T2/T3 × budget/mid/premium = 200/250/500, 400/500/900, 800/900/1600
- Smoking: ~20/50/350 per unit/pack
- Home-cook baseline: ~₹80/meal; delivery-vs-homecook savings ₹120–300/day

---

## Summary table

| Finding | Verdict | Consequence for Bounce |
|---|---|---|
| Feb 1, 2026 tobacco excise overhaul added ₹2,050–₹8,500 excise duty per 1,000 sticks (on top of 40% GST), pushing retail cigarette prices up ~₹22–55/pack of 20 nationally | **Confirmed** `[verified — TaxGuru report of Central Excise notification, effective 1 Feb 2026]` + corroborated `[verified — multiple secondaries: Deccan Herald, WION, Angel One, A2Z Taxcorp]` | Smoking placeholder (~₹20/50/350) is now **stale**; real per-stick cost has a much wider spread by brand than one flat number can capture |
| Live retail (quick-commerce) prices, July 2026: Gold Flake Premium 10-pack ₹99 (~₹9.9/stick); Gold Flake Filter 10-pack ₹117 (~₹11.7/stick); Classic Milds 10-pack ₹240 / 20-pack ₹480 (~₹24/stick) | **Confirmed** `[verified — Zepto product listings; Blinkit + Swiggy Instamart product listings, cross-checked]` | Real range is **₹10–24/stick** depending on brand tier — placeholder's "20" (Tier1) sits mid-pack, not at the budget end; need brand-tiered logic, not a single number |
| Budget brands (Four Square, Bristol, Wills Navy Cut) reportedly ₹11–18/stick, ₹190–320 per 20-pack | **Unverified / Likely (weak)** `[hypothesis — sourced from aggregator blogs (digitalgriot, insidermonkey, duniakagyan), not fetched or cross-checked against a live retailer listing]` | Directionally plausible (below Gold Flake/Classic), but don't hard-code without a live-listing check |
| A pack described as "Classic Connect" hit ₹350 post-hike (per one blog); Classic Ultra Mild 20-pack ₹480 (Blinkit) | **Likely** `[verified — Blinkit product listing for Classic Ultra Mild]` / `[hypothesis — gstclub.in article, fetch blocked by expired TLS cert, only search-snippet available]` | Placeholder's Tier3 "350" for smoking is in the right zone for a premium 20-pack post-hike, but bracket definition (per-stick vs per-pack) needs to be made explicit in the doc |
| Mid-range Bangalore brewpub (Byg Brewski, Sarjapur Rd) food items ₹215–465/plate; site-quoted "cost for two" ≈ ₹1,500 (≈₹750/person, food only, no drinks priced) | **Confirmed (food only)** `[verified — Byg Brewski menu via Magicpin, fetched 2026-07-12]` | Confirms mid-tier food spend; drink prices weren't listed on this page — see next row |
| Toit Brewpub (Indiranagar) cocktails ₹200–1,150 (mocktails ₹200; signature cocktails ₹400–700; premium spirits to ₹1,150); food ₹150–850 | **Confirmed** `[verified — Toit menu via ExploreBangalore aggregator, fetched 2026-07-12]` | A 2–3 drink + food night at a premium Indiranagar venue plausibly lands ₹1,500–3,000+/person — roughly matches placeholder's premium row (1200/2500/5000) but no pint-specific figure found (breweries serve house beer without a single listed "pint price") |
| General nightlife cost estimates (blog aggregation): neighborhood pubs (HSR/BTM/Jayanagar) pint/cocktail ₹150–300; mid-range pubs ₹250–500/drink; upscale bars ₹600–1,000+/drink; full night out ₹2,000–7,000/person; Indiranagar specifically ₹1,500–2,500/person | **Unverified / Likely (weak)** `[hypothesis — cost-of-living/nightlife blogs (bohothebar, bangaloreblogs, ff21), not primary venue pricing, ranges very wide]` | Broadly consistent with placeholder's shape (budget < mid < premium, escalating with venue), but these are guide-book style estimates, not verified per-venue bills — treat placeholder as "reasonable order of magnitude," not confirmed |
| Zomato reported average order value (AOV) ≈ ₹425 in Q1 FY25 (across all food delivery, blended) | **Unverified** `[hypothesis — appears in search-engine synthesis attributed to Zomato's Q1FY25 shareholder letter; direct fetch of blog.zomato.com/q1fy25 and the Eternal/Zomato investor-relations PDFs failed (timeout / unreadable image-based PDF) — could not independently confirm the figure or its exact definition]` | Do not hard-code ₹425 as ground truth; it's a plausible blended national AOV but unconfirmed by direct reading, and Bangalore-specific AOV wasn't found anywhere |
| Quick-commerce (not restaurant food delivery) AOVs: Blinkit ₹665, Instamart ₹527 (FY25) | **Confirmed (different category)** `[verified — reported in search synthesis of company disclosures; consistent with widely-cited FY25 quick-commerce unit economics]` — but **flagged out of scope**: this is grocery quick-commerce, not Swiggy/Zomato *restaurant* food delivery | Don't conflate with food-delivery order value — Bounce's "food delivery" vice logging is about restaurant meals, a different spend pattern than 10-min grocery orders |
| Generic AOV bands cited in blog analysis: "light users ~₹250, moderate users ~₹300 per order"; example restaurant-level comparison "Zomato ₹650 vs Swiggy ₹480" for one premium restaurant | **Unverified** `[hypothesis — restrologic.com blog, presented as illustrative examples not sourced to a named dataset]` | Suggests real single-order values cluster ₹250–650 depending on restaurant tier and user segment — broadly brackets the placeholder's Tier1/Tier2 (200/250/500, 400/500/900) reasonably, but no authoritative single figure exists |
| Home-cook ingredient cost, computed from verified retail unit prices (Sona Masoori rice ₹62–94/kg; toor dal ₹122–159/kg; Fortune sunflower oil ₹195–200/L; onion/tomato ₹22–33/kg) → a simple rice+dal+one sabzi+oil portion computes to roughly ₹25–40/portion in raw ingredients alone | **Verified inputs, hypothesis for the total** `[verified — BigBasket product listings for rice/dal/oil (search-indexed prices, not independently re-fetched live); verified — mandi price trackers rozkabhav.com/oneindia.com for vegetables, dated Apr–May 2026]` + `[hypothesis — the ₹25–40/portion total is Bounce's own arithmetic, not a published figure, and excludes LPG/electricity, protein, condiments, wastage, and any labor cost]` | The venture doc's ₹80/meal assumption is **plausible and likely on the generous side** for a bare-ingredients meal, but becomes reasonable once LPG, protein, condiments and real-world wastage are added — recommend the doc state its ₹80 figure is "fully-loaded" not "raw ingredients," or it will look inflated next to this bottom-up math |
| Bangalore monthly tiffin/subscription services: ₹2,500–₹7,000/month depending on provider and veg/non-veg, i.e. roughly ₹85–235/meal for a *prepared, delivered* meal (not self-cooked) | **Confirmed (as a different baseline)** `[verified — multiple tiffin-service pricing pages: Maa Ka Dulaar, Bangalore Tiffin, Tiffyy, Dabba Meals, Growfit, cross-checked, consistent range]` | This is a *paid-tiffin* cost, not a *self-cooked* cost — useful as an upper bound / alternative baseline if Bounce ever models "outsourced home-style food" as a third option, but should not be conflated with the ₹80 self-cook assumption |
| Implied delivery-vs-homecook savings: if a typical restaurant delivery order runs ₹250–650 (unverified AOV bands above) against a self-cooked meal at ~₹25–80 (ingredient math + doc assumption), the gap is roughly ₹170–570/day | **Hypothesis (derived, not directly sourced)** `[hypothesis — Bounce's own arithmetic combining two already-unverified/weakly-verified inputs]` | The doc's claimed ₹120–300/day savings range is **plausible and may even be conservative** at the higher end, but rests on two shaky inputs (AOV, ingredient cost) — don't upgrade this range to "confirmed" without a cleaner primary source for at least one side of the comparison |

---

## Detail per finding

### 1. Cigarettes

**Tax change (the headline driver of 2026 pricing).** TaxGuru's report of the Central
Excise notification set (fetched directly) gives concrete post-Feb-1-2026 excise duty
rates per 1,000 sticks: non-filter ≤65mm ₹2,050, filter ≤65mm ₹2,100, filter 65–70mm
₹4,000, filter 70–75mm ₹5,400, filter >75mm ₹8,500 — layered on top of the pre-existing
40% GST (which itself replaced the 28%+cess regime as compensation cess lapsed).
[verified — TaxGuru, https://taxguru.in/excise-duty/tobacco-excise-duty-slabs-overhauled-notification.html]

Multiple news secondaries (Deccan Herald, WION, Angel One, A2Z Taxcorp) converge on the
same February 1, 2026 effective date and describe pack-level price increases of
roughly ₹22–55 for a 20-pack, though I could not fetch these articles directly
(403 errors on Deccan Herald, WION; both only available as search-engine summaries).
[hypothesis — search-engine synthesis of Deccan Herald https://www.deccanherald.com/business/union-budget/union-budget-2026-cigarette-prices-surge-up-to-rs-55-per-pack-after-excise-duty-hike-3882605 and WION https://www.wionews.com/india-news/how-much-will-your-cigarette-cost-from-feb-1-beedi-pan-masala-to-get-costlier-as-centre-imposes-new-excise-duty-cess-1767254899124 — treated as Likely given TaxGuru's independent, directly-fetched corroboration of the mechanism]

**Live retail prices (July 2026), fetched directly from quick-commerce product pages:**
- Gold Flake Premium, 10 sticks: ₹99 (~₹9.9/stick) — [verified — Zepto, https://www.zepto.com/pn/gold-flake-premium-cigarettes/pvid/7d3fcb4b-70c2-4b24-9382-4d95c3675538]
- Gold Flake Filter, 10 sticks: ₹117 (~₹11.7/stick) — [verified — Zepto, https://www.zepto.com/pn/gold-flake-filter-cigarette/pvid/05776d06-bbbe-4344-91a3-e6e78118529d]
- Classic Milds (Balanced Taste), 10 sticks: ₹240 (~₹24/stick) — consistent across two independent platforms — [verified — Blinkit https://blinkit.com/prn/classic-mild/prid/503582 ; Swiggy Instamart https://www.swiggy.com/instamart/pc/classic-milds-7KPS6P1VG2]
- Classic Milds, 20 sticks: ₹480 — [verified — Blinkit https://blinkit.com/prn/classic-mild/prid/479280]
- Classic Ultra Mild, 20 sticks: ₹480 — [verified — Blinkit https://blinkit.com/prn/classic-ultra-mild/prid/479281]

**Budget-brand claims (not independently verified):** Four Square ₹12–18/stick,
₹190–320/20-pack; Bristol ₹11–17/stick, ₹180–300/20-pack; Wills Navy Cut ~₹120/10 sticks.
These come from aggregator/review-style blogs (digitalgriot.com, insidermonkey.com,
duniakagyan.com) that were not fetched directly and show no cited primary source
themselves. [hypothesis]

**Contradiction noted:** one search synthesis stated "Gold Flake now costs ₹240 for a
pack of 10, ~₹24/stick" — this does not match the live Zepto listings I fetched
(₹99–117 for Gold Flake variants). The ₹240/10-stick, ₹24/stick figure actually matches
**Classic Milds**, not Gold Flake, in the live listings. This looks like the AI search
synthesis conflating two ITC brands. Flagging as a contradiction rather than silently
picking one: **Classic Milds retails at ₹24/stick; Gold Flake retails at ₹9.9–11.7/stick**,
per the two live listings fetched directly.

### 2. Alcohol

No live drink-menu price list was found for a "budget" venue tier (the venture doc's
₹300–1200 budget row) — search results only characterized budget/neighborhood pubs
qualitatively ("HSR/BTM/Jayanagar pint or cocktail ₹150–300") via cost-of-living blogs,
not a specific venue's menu. [hypothesis — bohothebar.com, bangaloreblogs.com, ff21.in]

For mid/premium tier, two actual venue menus were fetched:
- **Byg Brewski (Sarjapur Rd)**: food ₹215 (half-plate kebab) to ₹465 (prawns ghee
  roast, full plate); "cost for two" listed as ~₹1,500 (~₹750/head), food only — beer/
  cocktail prices were not visible on the fetched menu page. [verified — Magicpin,
  https://magicpin.in/Bangalore/Sarjapur-Road/Restaurant/Byg-Brewski-Brewing-Company/store/6c76/menu/]
- **Toit (Indiranagar)**: mocktails ₹200; signature cocktails ₹400–700; premium
  spirit-based drinks up to ₹1,150; food ₹150–850 (appetizers ₹150–500, pizzas
  ₹500–725, large plates ₹375–850). [verified — ExploreBangalore aggregator,
  https://www.explorebangalore.com/breweries/toit-brewpub/menu]

Neither venue's page listed a plain "pint of house beer" price — breweries sell beer
in various formats (tasters, growlers, pitchers) rather than a single standard pint
SKU, which made this harder to pin down than expected.

Putting 2–3 drinks (₹400–700 each at Toit) + food (₹300–500/head) together gives a
plausible premium-venue per-person spend of ₹1,500–3,000+, which brackets the
placeholder's premium row (1200/2500/5000) reasonably. This is triangulation, not a
verified single receipt.

### 3. Food delivery

Attempts to pull Swiggy's and Zomato's own investor-relations AOV disclosures directly
failed: the Swiggy Q3FY25 shareholder-letter PDF and the Eternal (Zomato) Q1FY26
shareholder-letter PDF are both image-based/infographic PDFs that render as
unreadable binary when fetched, and a direct fetch of blog.zomato.com/q1fy25 timed
out twice. [attempted, both failed — see search trail below]

A search-engine synthesis (not a direct read) attributed "₹425 average order value in
Q1 FY25" to Zomato's shareholder letter. I could not independently confirm this
number or its exact scope (blended across food delivery only, or including quick
commerce). [hypothesis]

Swiggy's DRHP (2024 pre-IPO filing) is reported, via search synthesis of Outlook
Business/Business Today coverage, to show GOV of ~₹35,000 cr across ~14.3M monthly
transacting users for the consumer business (food delivery + Instamart + dining) —
this is a blended figure across business lines, not a clean food-delivery AOV.
[hypothesis — https://www.outlookbusiness.com/start-up/explainers/what-swiggys-ipo-filing-reveals-about-its-bid-to-take-on-zomato]

Quick-commerce AOVs (Blinkit ₹665, Instamart ₹527, FY25) are a **different product
category** (10-minute grocery delivery) from restaurant food delivery — flagging this
explicitly so it doesn't get conflated with Bounce's "food delivery vice" logging,
which is about restaurant meal orders.

No Bangalore-specific or single-item (e.g., "typical biryani order") price was found;
searches for specific delivery menu prices (biryani, thali) returned only restaurant
directory listings with no visible prices in the fetched search snippets.

### 4. Home-cooked meal baseline

Verified per-unit retail ingredient prices (BigBasket, search-indexed product listing
prices, not independently re-fetched live at full page load):
- Sona Masoori rice, 5kg: ₹310–470 → ₹62–94/kg [verified — BigBasket product listings,
  e.g. https://www.bigbasket.com/pd/40128964/bb-popular-new-rice-sona-masoori-5-kg/]
- Toor dal, 1kg: ₹122–159 [verified — BigBasket, e.g.
  https://www.bigbasket.com/pd/10000425/bb-royal-toor-dalarhar-dal-desi-1-kg-pouch/]
- Fortune sunflower oil, 1L: ~₹195–200 [verified — pricee.com/BigBasket cross-check,
  https://pricee.com/fortune-sunlite-refined-sunflower-oil-price-in-india-94545]
- Onion/tomato: ₹22–33/kg retail, Bangalore, Apr–May 2026 mandi trackers [verified —
  rozkabhav.com, oneindia.com]

Bounce's own arithmetic (not a published figure): a single portion of rice + dal +
one vegetable + oil, at these unit prices, computes to roughly ₹25–40 in raw
ingredients. This excludes LPG/electricity, protein (egg/chicken/paneer), spices,
condiments, real-world wastage, and any value for time/labor. [hypothesis — derived
calculation]

For context, actual **paid tiffin services** (fully prepared, delivered meals) in
Bangalore run ₹85–235/meal depending on provider and veg/non-veg
[verified — multiple tiffin service pricing pages: maakadulaar.com, lytmeals.com,
tiffyy.com, cross-checked, consistent range ₹2,500–7,000/month].

**Read on the ₹80/meal placeholder:** it sits comfortably above the bare-ingredients
floor (₹25–40) and well below the paid-tiffin ceiling (₹85–235) — consistent with a
"fully-loaded home-cook cost including LPG, protein, and waste" interpretation, but
the venture doc doesn't currently state that scope, so the number reads as either
generous (if meant as raw ingredients) or realistic (if meant as fully-loaded). This
distinction should be made explicit in the doc.

---

## What we could NOT verify

- **Zomato/Swiggy Bangalore-specific average order value.** Searched: "Swiggy Zomato
  average order value Bangalore 2026", "Zomato food delivery average order value
  annual report FY25", direct fetch of blog.zomato.com/q1fy25 (timed out twice),
  direct fetch of Swiggy Q3FY25 and Eternal Q1FY26 shareholder-letter PDFs (both
  rendered as unreadable image-based binary). Found instead: a search-engine-synthesized
  claim of "₹425 in Q1FY25" with no independently-confirmed primary reading, and two
  blended, non-Bangalore-specific company-wide GOV/MTU figures. What would settle it:
  a text-native copy of Zomato's or Swiggy's quarterly investor deck (not the
  infographic-style PDF), or a city-level breakout, which neither company appears to
  publish externally.

- **A single named budget-tier bar/pub's actual drink menu** (Indiranagar/HSR/Koramangala).
  Searched several specific venue names; only broad cost-of-living blog estimates
  turned up for the budget tier, not an actual fetched menu. What would settle it: a
  Zomato/Dineout venue page with itemized drink prices for a specifically budget
  (not brewery/gastropub) venue — several of these blocked WebFetch (Zomato pages
  returned 403 or were not fetched in this pass).

- **Cheap cigarette brand pricing** (Four Square, Bristol, Wills Navy Cut) from a
  live retailer listing. Searched multiple queries; only got aggregator-blog price
  claims with no visible sourcing of their own. What would settle it: a live
  Zepto/Blinkit/Swiggy Instamart listing for these specific SKUs, the same way Gold
  Flake and Classic were confirmed.

- **The primary CBIC notification / PIB text itself** for the Feb 2026 tobacco excise
  hike. I read TaxGuru's report of it (which reproduces the rate schedule) but did
  not fetch the government's own notification or PIB release directly. What would
  settle it: the actual CBIC notification number and text (searches surfaced titles
  like "New Duties On Tobacco Products Notified" on taxtmi.com and A2Z Taxcorp, but
  these weren't fetched in this pass either).

## Out of scope, noticed

- Quick-commerce (Blinkit/Instamart) unit economics — AOV, delivery-partner pay per
  order — surfaced repeatedly in food-delivery searches but is a different product
  category from restaurant food delivery; not folded into the table above beyond one
  explicit flag.
- Swiggy's per-order delivery-partner payout (~₹56/order, FY24, per DRHP-derived
  search synthesis) — interesting for a future "gig worker cost" angle but irrelevant
  to Bounce's consumer-facing budget reflow; noted only, not verified.

## Suggested next questions

1. Get a text-native (not image PDF) copy of Zomato's or Swiggy's latest investor
   letter, or find a press article that quotes AOV with an explicit citation to page/
   line — needed before the food-delivery placeholder can move past "hypothesis."
2. Pull 3–5 actual Zomato/Swiggy restaurant menu pages for specific Bangalore budget
   venues (not breweries) to nail the alcohol budget tier and a "typical single meal
   order" figure for food delivery — this pass's WebFetch attempts on Zomato pages
   were blocked (403); worth retrying with a different fetch path or checking
   Dineout/magicpin equivalents.
3. Decide, in the venture doc, whether the ₹80/meal home-cook baseline is meant as
   raw-ingredients-only or fully-loaded (LPG, protein, waste) — this changes whether
   ₹80 needs revising up or down, and this ambiguity should be resolved before the
   next doc revision rather than left implicit.
4. If Bounce wants brand-tiered smoking costs (rather than one flat number), the
   Gold Flake (₹10–12/stick) vs Classic (₹24/stick) spread found here suggests at
   least a 2-tier split is warranted, with the budget tier still needing live-listing
   verification.
