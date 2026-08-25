# UK Property Intelligence System
## Intent & Thinking Plan — v0.2

**Document type:** Pre-build thinking artefact / project constitution
**Status:** Open discussion, revised after v0.1 critique
**Research snapshot:** 25 August 2026
**Primary scope:** Edinburgh, Scotland — owner-occupier acquisition
**Stage:** Still before 0 → 1, but with a named first slice
**Not yet:** Implementation plan, stack decision, schema, model specification

---

# 0. What Changed From v0.1, and Why

v0.1 was a good map of the *ambition*. It was a poor map of the *constraints*.

Six things were either wrong, missing, or internally contradictory. v0.2 exists to correct them.

| # | v0.1 position | v0.2 position |
|---|---|---|
| 1 | Edge comes from finding repeatable small margins | Property transactions are too infrequent for statistical edge. Repeatability must be relocated. |
| 2 | Core data is free and open | True for England. **False for Scotland at transaction level.** Scotland-first has a real price tag. |
| 3 | §8.2 listed listing-duration, price-reduction, relisting metrics | Those live only in portal data, which §14 forbids obtaining. **Internal contradiction.** |
| 4 | Ten kinds of gap, all favouring the analyst | No section asked where the analyst is structurally *disadvantaged*. |
| 5 | Valuation analysis treated as sufficient | Scotland prices via **offers-over + closing date sealed bid**. Knowing more does not mean winning. |
| 6 | "Preserve optionality before optimising" (Appendix C) | Correct for models. **Wrong for the archive** — history cannot be reconstructed later. |

Plus one change of purpose, from the owner:

> The near-term objective is not investment. It is **buying a first home to live in, in Edinburgh, with a likely move every 8–10 years.**

That single change removes more than half of v0.1's complexity. It is the most important edit in this document.

**v0.2 is deliberately shorter than v0.1.** A thinking document that grows while the problem shrinks is a warning sign.

---

# 1. Revised Core Intent

## 1.1 The honest objective function

The system exists to answer one question well, and a second question later:

**Now (next 12–24 months):**

> Given what is knowable and lawfully obtainable, which Edinburgh property should I buy to live in, how much should I offer, and which properties should I refuse to bid on at any price?

**Later (5–15 years, serial owner-occupier):**

> When should I move, what should I sell into, and how should the capital and financing be structured across a sequence of homes rather than a single purchase?

**Not now, possibly never:**

> Where is the alpha in UK property?

## 1.2 Why the objective changed shape

For a serial owner-occupier the return has three components, and they are not equally tractable:

```text
TOTAL OUTCOME
     │
     ├── national / city price beta ............ not controllable, not predictable
     ├── entry timing in the rate cycle ........ 2–3 decisions in a lifetime, low sample
     ├── quality of life per pound ............. real, large, and mostly non-quantitative
     └── avoidance of specific bad outcomes .... CONTROLLABLE, DATA-ADDRESSABLE, VERIFIABLE
```

The fourth line is where a data system earns its keep.

A single bad purchase — an unmortgageable flat, a £35k common roof liability, a conservation-area property with restricted retrofit, a micro-market with no exit liquidity — destroys more value than a decade of clever area selection creates.

**Therefore v0.2 reframes the system from an alpha engine to a downside-screening and decision-acceleration engine.**

This is not a downgrade. It is the version that is provable in the available time with the available data.

## 1.3 The value test

The system is worth building if, within 12 months, it can honestly say:

> "I did not buy property X, for this specific documented reason, and that reason turned out to be correct."

If it cannot produce that sentence, no amount of architecture redeems it.

---

# 2. What v0.2 Excludes, and Why

This is the most important section in the document. v0.1 excluded nothing.

## 2.1 Excluded outright

### Short-term rental / Airbnb as a strategy
Edinburgh designated the **entire council area** a short-term let control area effective 5 September 2022. Using a dwelling that is not the host's principal home as an STL is deemed a material change of use requiring planning permission.
(City of Edinburgh Council — https://www.edinburgh.gov.uk/planning-13/short-term-lets-planning)

FOI data obtained by Gilson Gray and reported by Scottish Housing News in November 2024 indicated roughly 566 refusals out of 632 applications. A 5% visitor levy applies to the first five paid nights from 24 July 2026.

**Verdict:** the forward strategy is closed. Airbnb data is retained only as a *historical natural experiment* (see §9.3), not as an input to any purchase decision.

### Buy-to-let economics
ADS is 8% of the full purchase price for additional dwellings, applied flat rather than marginally, for contracts entered into on or after 5 December 2024. The 2026–27 Scottish Budget confirmed rates unchanged.
(Revenue Scotland — https://revenue.scot/taxes/land-buildings-transaction-tax/additional-dwelling-supplement-ads)

**Verdict:** not relevant to a first, sole, owner-occupied purchase. Deferred, not deleted — see §2.3.

### Rent control modelling
The Housing (Scotland) Act 2025 received Royal Assent on 6 November 2025. The rent control framework commenced 1 April 2026, but local authority rent condition assessments are not due until 31 May 2027, so no rent control area can exist before then.
(gov.scot — https://www.gov.scot/policies/private-renting/rent-controls/)

**Verdict:** irrelevant to owner-occupation. Retained as a *dated future regime change* for the §2.3 deferred layer.

### Purchased transaction-level data
Registers of Scotland sells sales data under licence: market-value residential sales at £675 + VAT per month for all Scotland, all-sales data at £933 + VAT per month, with per-county pricing available.
(RoS — https://www.ros.gov.uk/data-and-statistics/sales-and-bespoke-data/sales-data-reports)

**Verdict:** excluded for now. An owner-occupier does not need bulk comparables; they need a handful of comparables per candidate property, obtainable free case-by-case. Revisit only if the project ever moves to systematic area analysis.

### Portal scraping
ESPC, Rightmove and Zoopla prohibit automated bulk extraction and expose no public API. Everything in v0.1 §8.2 that depended on listing-level history was therefore unobtainable within v0.1's own stated legal boundary.

**Verdict:** the constraint is accepted, and the workaround is redesigned in §7.3.

### A single composite "good area score"
v0.1 already argued against this. v0.2 upgrades it from preference to prohibition. A score hides the reason, and the reason is the product.

## 2.2 Excluded because the data does not exist at useful granularity

### Street-level crime in Scotland
Police.uk does not cover Scotland. Police Scotland publishes aggregate and management-information statistics, not street-level incident data with an API. Any crime analysis will therefore be coarse (roughly council/ward level) and not comparable with the English data v0.1 assumed.

**Verdict:** accept the gap. Use SIMD crime domain as the available proxy, at Data Zone level, and stop pretending finer resolution is available.

### Common repair liability
The single largest unpriced financial risk in an Edinburgh tenement purchase — shared roof, stonework, stair, common drainage — appears in **no open dataset anywhere**. It exists in the Home Report condition ratings, the factor's records, and neighbours' knowledge.

**Verdict:** this is handled by process, not data. See §5.

### Street lights, water, sewerage, granular utility data
v0.1 listed these. They are either unavailable, unmaintained, or have no plausible mechanism connecting them to an owner-occupier decision at the margin.

**Verdict:** removed from the candidate universe. They can return if a mechanism is ever articulated.

## 2.3 Deferred, not deleted

The following stay in the long-run picture but contribute nothing to the first purchase:

- second-property / buy-to-let economics and ADS sequencing
- rent control area designation once assessments land (post-May 2027)
- UK-wide data architecture
- England / Wales / Northern Ireland jurisdictions
- commercial or mixed-use property
- development, subdivision, change of use
- machine learning of any kind
- any notion of productising, licensing or consulting

**On the last point specifically:** designing for a future product biases the system toward being general, complete and architecturally clean. Designing for one real purchase in Edinburgh biases it toward being ugly, specific and correct. These two pressures are in direct conflict at this stage. v0.2 chooses correct.

---

# 3. What v0.2 Keeps From v0.1

Not everything in v0.1 was wrong. The following survive, with changed rank.

| v0.1 pillar | v0.2 status |
|---|---|
| **Time Machine** | **Promoted to first place.** The only time-irreversible asset. See §6. |
| **Liquidity** | **Promoted.** For an 8–10 year horizon, exit liquidity is a primary criterion, not a secondary metric. |
| **Common Property Kernel** | Retained but minimised. For one city and one buyer, the kernel is: UPRN ↔ address ↔ postcode ↔ Data Zone. Nothing more until something breaks. |
| **Law / Tax** | Reduced to three live items: LBTT and first-time buyer relief, council tax band, and the ADS sequencing decision at the point of the *next* move. |
| **Data Ball** | Demoted from ambition to constrained shortlist. See §8. |
| **Market Bug** | Retained as a research discipline (the §4.3 hypothesis structure was good), but no bug is assumed to exist. |
| **Leverage** | Reframed: not an amplifier of edge, but a **survivability constraint**. What matters is stress capacity, not maximisation. |

---

# 4. New Layer: The Pricing Mechanism

v0.1 analysed valuation. It never analysed **how price is actually set in Scotland**, which is a different thing.

## 4.1 Offers over and closing dates

The Scottish process is not negotiation. A property is marketed at an "offers over" figure. Interested buyers note interest. If competition is sufficient, the selling agent sets a closing date, and offers are submitted blind.

In a sealed-bid auction, the winner is the highest bidder — not the best-informed bidder.

```text
Your model says property is worth £X
        │
        ▼
You bid £X (or slightly under)
        │
        ├── Others bid less  ──► you win, but the property was not contested,
        │                        which is itself information about its desirability
        │
        └── Others bid more  ──► you lose
                                 │
                                 ▼
        You systematically win only the properties nobody else wanted
                        = adverse selection
```

**This is the winner's curse, and it is the central execution risk of the entire project.**

Any information advantage that is *also visible to other buyers* gets competed away at the closing date. The advantage must therefore live in one of three places:

1. **Things others cannot easily value** — common repair exposure, retrofit cost under conservation restrictions, factor quality, long-horizon exit liquidity.
2. **Things others do not bother to check** — £/m² from EPC floor area, Data Zone transaction volume, catchment boundary edges.
3. **Discipline** — a pre-committed maximum bid, and the willingness to lose.

The third is the largest and the hardest. A system that produces a number you then exceed emotionally has produced nothing.

## 4.2 The Home Report as the local pricing primitive

Scotland has something England does not: **every marketed property carries an independent surveyor's valuation**, provided by the seller.

This creates a free, universal, third-party anchor. The ratio of achieved price to Home Report valuation is the local competition thermometer, and ESPC publishes it monthly at no cost.

Recent published figures (ESPC House Price Report, https://espc.com/news):

| Period | % of Home Report achieved | Median time to under offer | % going to closing date |
|---|---|---|---|
| Feb–Apr 2026 | 101.2% | 29 days | 17.3% |
| Mar–May 2026 | 101.7% | 25 days | 18.6% |
| Quarter to Jun 2026 | 102.1% | — | — |

ESPC also reported that 72.8% of properties in the Feb–Apr 2026 period sold at or above Home Report valuation, and that in the Q1 2026 prime market EH12 achieved the highest ratio at 103.2%.

**This is the single most decision-relevant free dataset available, and v0.1 did not mention it.**

---

# 5. New Layer: Where I Am the Disadvantaged Party

v0.1 enumerated ten gaps, all of which favoured the analyst. This is an availability bias — gaps that favour you are the ones you think of.

The honest inverse list:

| Dimension | Who holds the advantage | Can data close it? |
|---|---|---|
| Common repair history, roof/stone condition, upcoming scheme costs | Neighbours, factor, local solicitors | **No.** Home Report + factor enquiry + asking. |
| Number and strength of competing notes of interest | Selling agent | **No.** Partially inferable from time on market. |
| Seller motivation and urgency | Selling agent | **No.** |
| Which streets flood, which stairs have damp, which blocks have antisocial issues | Long-term residents | **No.** Visit at different times, talk to people. |
| Realistic renovation cost in a conservation area | Local trades | **No.** Get quotes before bidding, not after. |
| Which agents systematically under- or over-set offers-over prices | Repeat buyers, solicitors | **Partially** — your own funnel data, over time. |

**Implication for the project:** roughly half of the decision-critical information in an Edinburgh purchase is non-digital and will never be in the system. The system's job is to handle the half it can, fast and reliably, so that human attention is free for the half it cannot.

This should be stated on the front page of whatever gets built, because it is the fact most likely to be forgotten once the data starts flowing.

---

# 6. The Irreversibility Asymmetry

v0.1 Appendix C said: preserve optionality in thinking before optimising implementation.

That is correct for models, schemas, scoring and architecture. **It is wrong for the archive.**

```text
MODELS                          ARCHIVE
can be changed later    vs      cannot be reconstructed later
delay costs nothing             delay costs one day, permanently, per day
```

Most UK open data publishes only the current version. Superseded versions are usually gone. Listings vanish when they sell. ESPC monthly reports are replaced. EPC records are amended.

**Conclusion: the only thing that must start immediately is dumb, unparsed, timestamped capture. The only thing that should wait is every interpretation of it.**

This resolves the tension without abandoning the v0.1 principle. It also happens to mean the first build requires zero architectural decisions, which is why it can start before this document is finished being argued with.

---

# 7. The Data Reality

Three honest tiers, replacing v0.1's five.

## 7.1 Free, obtainable, useful now

| Source | What it gives | Cadence | Why it matters here |
|---|---|---|---|
| Home Report (per property) | Surveyor valuation, condition ratings 1/2/3, floor area, EPC | Per case | The anchor for every bid |
| Scottish EPC register | Floor area, build era, heating, rating | Quarterly bulk | **Free £/m² denominator** — most buyers only see total price |
| Scottish Assessors (SAA) | Council tax band by address | Static | Ongoing cost + crude historic value banding |
| ESPC monthly House Price Report | HR premium, time to under offer, closing-date rate, by sub-area | Monthly | The competition thermometer |
| UK HPI (Scotland) | Index by local authority | Monthly | Cycle context |
| Registers of Scotland free statistics | Monthly and small-area volumes and values | Monthly | **Transaction volume = liquidity proxy** |
| SIMD | Deprivation domains at Data Zone level | Multi-year | Best available crime/health/access proxy |
| statistics.gov.scot / NRS | Population, households, tenure at Data Zone | Varies | Slow-moving demand context |
| Historic Environment Scotland | Listed buildings, conservation areas | Rarely | **Directly drives future maintenance cost and retrofit restriction** |
| SEPA flood maps | Flood risk | Rarely | Insurability, mortgageability |
| Edinburgh Council planning portal | Applications near the property | Weekly | What is about to be built next door |
| Edinburgh Council STL licence register | Which flats in the stair are short-let | Periodic | Living quality + block character |
| Edinburgh Council school catchments | Catchment boundaries | Annual | **Resale demand at the 8–10 year exit** |
| Ofcom Connected Nations | Broadband/mobile by postcode | Semi-annual | Baseline liveability check |
| OS Open UPRN / Open Roads | Identifiers, geometry | Periodic | Joins everything else together |

## 7.2 Paid — consciously declined

- RoS transaction-level sales data (£675–£933 + VAT / month)
- Portal data licences (not available to individuals at any realistic price)
- Commercial aggregators

**Position:** declined for the owner-occupier phase. Per-property comparables can be looked up free, case by case, at the point of decision. Bulk is a requirement of area *research*, not of buying one home.

## 7.3 Prohibited — and the lawful substitute

Automated bulk extraction from ESPC/Rightmove/Zoopla is off the table.

The lawful substitute is a genuine change of unit of analysis:

```text
v0.1 assumed:  the whole market, scraped
v0.2 accepts:  MY funnel, recorded

30–80 properties per month that I actually consider
= 400–900 observations per year
= a longitudinal dataset that exists nowhere else
= and it is exactly the population my decision is drawn from
```

This is smaller, slower, and better suited to the question. It also cannot be bought, which makes it the only genuinely proprietary asset in the project.

---

# 8. The Constrained Data Ball

v0.1's Appendix B listed roughly 95 candidate domains. v0.2 keeps the list as a *boundary of curiosity* but adds an admission rule:

> A dataset enters the working set only when a named decision would change if its value changed.

Applied to the first-home decision, the working set is roughly 15 sources (§7.1), not 95.

Everything else stays in Appendix B as a parking lot. Parking is not rejection. But a parked dataset costs nothing, and an ingested dataset costs maintenance forever.

**Maintenance is the hidden budget line v0.1 never mentioned.** Fifteen sources with different schemas, revision cycles and licences is already a standing commitment. Ninety-five is a full-time job that produces no decisions.

---

# 9. The End Big Picture

Staged, so that each stage is independently worth reaching.

## 9.1 Stage 1 — Personal decision system (0–24 months)

A private archive plus a personal funnel record, producing:
- a kill checklist that eliminates unsuitable properties fast
- a defensible maximum bid for each candidate, anchored on Home Report and local premium structure
- a written record of every property considered and why it was rejected

**Success looks like:** one purchase made with documented reasoning, and a folder of rejections with reasons that hold up in hindsight.

## 9.2 Stage 2 — Serial owner-occupier system (2–10 years)

The same structure, extended across a sequence of homes:
- exit timing informed by the liquidity and premium data now accumulated
- the ADS sequencing decision at the next move (keep and let vs sell and replace, with the 36-month replacement-of-main-residence refund rule in view)
- refinancing and product-transfer decisions, which unlike purchases *do* recur often enough to accumulate evidence

**This is where the "repeatable small margin" instinct from the original conversation actually lives.** Not in buying, in financing and timing.

## 9.3 Stage 3 — Research capability (optional, evidence-permitting)

Only if Stages 1–2 produce a real archive. Then the interesting questions become answerable, because by then there is point-in-time data that nobody else kept:

- Did the 2022 STL control area designation reprice Edinburgh flats, and where?
- Is the Home Report premium a stable area characteristic or a market-wide sentiment drift?
- Does time-to-under-offer lead price at Data Zone level?
- When rent control areas are designated post-2027, how fast does the market reprice?

Note that every one of these requires **history that must be captured starting now** to be answerable later. That is the entire argument of §6, restated as motivation.

## 9.4 Stage 4 — Anything commercial

Explicitly out of scope, explicitly not designed for, and revisited only if Stage 3 produced something surprising and true.

---

# 10. The First Thin Slice

Deliberately unimpressive. That is the point.

## 10.1 Three components, in order

**A. The dumb archive**
Timestamped raw capture of a handful of published sources. No parsing, no schema, no database. Files in dated folders. Started immediately, because §6.

**B. The funnel record**
One row per property genuinely considered. Minimum viable fields:

```
date_seen · address · postcode · data_zone · property_type · bedrooms
epc_floor_area · home_report_value · asking_price · date_listed
date_under_offer · went_to_closing_date · tenement? · conservation_area?
council_tax_band · factor_known? · subjective_score · reason_not_pursued
```

The last field is the most valuable in the document. It will be worth more in six months than any model.

**C. The kill checklist**
Not a score. A set of conditions that eliminate a property, each with a stated reason. Draft in Appendix B.

## 10.2 What the first slice must NOT include

- no database engine choice
- no dashboard
- no map
- no scoring formula
- no ingestion of anything from §8's parking lot
- no code that takes more than an afternoon to replace

If the slice takes more than a few evenings to stand up, it has already exceeded its remit.

---

# 11. The First Testable Hypothesis

One hypothesis, structured per v0.1 §4.3, chosen because it directly serves the live decision.

```text
OBSERVATION
Home Report premium varies by area and property type
(ESPC: whole region ~101–102%, EH12 prime 103.2% in Q1 2026)

POSSIBLE MECHANISM
Premium reflects local buyer competition density, which is a function of
supply (new listings), demand (affordability, catchment, commute),
and surveyor conservatism in that segment

WHO MIGHT BE MISPRICING IT
Buyers who apply a single citywide rule of thumb ("bid 5% over")
rather than a segment-specific one

WHEN DOES THE INFORMATION BECOME AVAILABLE
Monthly, publicly, free, from ESPC — before any individual bid is made

WHAT SHOULD HAPPEN IF TRUE
Relative premium ranking between sub-areas and property types
should be STABLE across months, not merely drifting together
with the market-wide average

WHAT WOULD DISPROVE IT
Sub-area premiums move in lockstep with the citywide figure.
Then premium is sentiment, carries no selection value,
and the correct response is to use the citywide average and stop.

DOES IT SURVIVE COSTS
Yes — this hypothesis costs nothing to test and, if true, changes the
single largest controllable number in the whole purchase: the bid.
```

Test window: six months of monthly ESPC reports plus the funnel record. No modelling required.

---

# 12. Success, Failure and Stopping

v0.1 asked what evidence would show no edge exists (Q8) but never answered it, and had no exit.

## 12.1 The benchmark

Every claim of usefulness is measured against a specific alternative, not against nothing:

> Buy a conventional flat in a demonstrably liquid Edinburgh sub-market, at the Home Report valuation plus the citywide average premium, with a conservative LTV, and do no analysis at all.

If the system cannot beat that, the system is a hobby. That is an acceptable answer, but it should be an acknowledged one.

## 12.2 Stopping conditions

The project stops, or is deliberately reduced to the archive alone, if:

- the §11 hypothesis is disproved and no replacement hypothesis is articulated within one month
- more than a stated number of hours per month goes to data maintenance rather than decisions
- six months pass with no property rejected for a documented, data-derived reason
- the archive is running but nothing is being read from it

The archive is the one component that should survive all of these, because it is cheap and irreversible.

## 12.3 The null result is a real result

If the honest conclusion is *"Edinburgh house prices at Data Zone level are not predictable from open data, and the correct strategy is to buy something liquid, structurally sound and pleasant to live in, at a disciplined price"* — that is a genuinely valuable output, and it is reached far faster by the thin slice than by the Data Ball.

---

# 13. Open Questions for v0.3

Reduced from thirty to nine. The rest were downstream of these.

**Decision**
1. What is the realistic budget, deposit and LTV, and what is the maximum monthly payment under a stressed rate?
2. What does the 8–10 year horizon actually imply — is exit liquidity or capital growth the binding criterion?
3. What are the non-negotiable liveability constraints (commute, space, noise, outdoor space), and are they being allowed to be traded away by data?

**Execution**
4. What is the pre-committed rule for maximum bid, and what makes it credible enough to survive a closing date?
5. Which segments have thin competition, and is that a signal of opportunity or of illiquidity?

**Data**
6. Which of the fifteen §7.1 sources would actually change a decision, and which are there because they exist?
7. What is the honest monthly time budget for maintenance?

**Sequencing**
8. At the next move, keep-and-let or sell-and-replace — and what does that imply for what is bought *now*?
9. What would have to be true for the paid RoS data to become worth £675+/month?

---

# 14. v0.2 Working Thesis

> Edinburgh's owner-occupier market prices through a sealed-bid mechanism anchored on an independent surveyor's valuation that every property carries. This makes the local competition premium observable and free, and makes information advantage partially self-defeating at the point of purchase.
>
> For a buyer making three to five purchases in a lifetime, no statistically validated edge is achievable. The achievable and provable value is in eliminating specific bad outcomes, bidding with pre-committed discipline, and accumulating a private, point-in-time record of a market that publishes only its present state.
>
> Roughly half of the decision-critical information — common repair liability, seller motivation, competing interest, local condition knowledge — is not in any dataset and never will be. The system's purpose is to dispatch the other half quickly and reliably enough that human attention is free for the half that matters most.
>
> The archive must start now because history cannot be recovered. Everything else can wait, and most of it should.

---

# 15. One-Page Mental Model — v0.2

```text
┌───────────────────────────────────────────────────────────────────────┐
│              EDINBURGH FIRST-HOME DECISION SYSTEM (v0.2)              │
│                                                                       │
│  Goal: buy one good home at a disciplined price, and avoid the        │
│        specific bad outcomes that data can actually detect.           │
├───────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ARCHIVE  (start today, no design decisions required)                 │
│  timestamped raw capture · ESPC reports · EPC · HPI · RoS stats       │
│                     │                                                 │
│                     ▼                                                 │
│  MY FUNNEL  (the only proprietary dataset)                            │
│  every property considered · HR value · asking · dates · why not      │
│                     │                                                 │
│                     ▼                                                 │
│  KILL CHECKLIST  (reasons, not scores)                                │
│  common repair · conservation · EPC · flood · liquidity · £/m²        │
│                     │                                                 │
│                     ▼                                                 │
│  BID DISCIPLINE                                                       │
│  Home Report anchor + segment premium + pre-committed ceiling         │
│                     │                                                 │
│                     ▼                                                 │
│  DECISION + WRITTEN REASON                                            │
│                                                                       │
│  NOT IN SCOPE: BTL · STR · rent control · England · scoring · ML      │
│  NOT OBTAINABLE: street crime · common repair history · seller intent │
│  NOT AFFORDABLE: RoS transaction data · portal licences               │
└───────────────────────────────────────────────────────────────────────┘
```

---

# Appendix A — Scotland-Specific Constraint Notes (August 2026)

Anchoring facts. Verify current status before relying on any of them for a real transaction.

**LBTT / ADS.** ADS is 8% of the full consideration for additional dwellings, contracts on or after 5 December 2024; charged flat, not marginally, which bites hardest at lower price points. The 2026–27 Scottish Budget kept LBTT rates and bands unchanged. First-time buyer relief raises the nil-rate threshold — confirm the current figure directly with Revenue Scotland.
https://revenue.scot/taxes/land-buildings-transaction-tax/additional-dwelling-supplement-ads

**Short-term lets.** Whole City of Edinburgh Council area is a control area from 5 September 2022. Secondary letting of a non-principal home requires planning permission. Visitor levy of 5% on the first five paid nights from 24 July 2026.
https://www.edinburgh.gov.uk/planning-13/short-term-lets-planning

**Rent control.** Housing (Scotland) Act 2025, Royal Assent 6 November 2025. Rent control framework in force 1 April 2026 via Commencement No. 3 Regulations 2026. Local authority rent condition assessments due by 31 May 2027; designations later. Caps in designated areas are CPI + 1 percentage point, hard ceiling 6%. Certain properties exempt.
https://www.gov.scot/policies/private-renting/rent-controls/

**Transaction data.** RoS sells sales data under licence from 2003; market-value residential £675 + VAT per month all-Scotland, all sales £933 + VAT, county splits available. No free bulk equivalent to England's Price Paid Data.
https://www.ros.gov.uk/data-and-statistics/sales-and-bespoke-data/sales-data-reports

**Market metrics.** ESPC publishes monthly, free, covering Edinburgh, Lothians, Fife and Borders: percentage of Home Report valuation achieved, median time to under offer, proportion going to closing date, with sub-area and prime-market breakdowns.
https://espc.com/news

**Crime.** Police.uk does not cover Scotland. No street-level incident API equivalent exists. Use SIMD crime domain at Data Zone level as the available proxy.

---

# Appendix B — Kill Checklist, First Draft

Conditions that eliminate or force escalation. Each carries a reason, and the reason is the output.

**Structural / cost**
- [ ] Home Report condition rating 3 on any *common* element → obtain factor records and recent scheme costs before proceeding
- [ ] Tenement with no factor or an inactive owners' association → repair decisions become unresolvable
- [ ] Roof or stonework with no recorded works in living memory → price in a major scheme
- [ ] Conservation area or listed → confirm what window, heating and insulation work is actually permitted, and cost it

**Regulatory / future cost**
- [ ] EPC F or G → future retrofit and resale exposure
- [ ] Flood risk indicated by SEPA → check insurability and mortgageability first, not last
- [ ] Live planning application adjacent that materially changes light, noise or outlook

**Liquidity / exit at 8–10 years**
- [ ] Very low transaction volume for this type and price band in this Data Zone → thin exit
- [ ] Property type unusual for the area → narrow buyer pool
- [ ] Price sits just above a well-known band boundary → buyer pool cliff
- [ ] Building or tenure feature likely to restrict mortgage availability

**Price**
- [ ] £/m² materially above comparable same-type properties in the same Data Zone → paying for finish that will not be recovered
- [ ] Required bid exceeds pre-committed ceiling → **walk, without exception**

**Human checks that no dataset performs**
- [ ] Visited on a weekday evening and a weekend
- [ ] Spoken to at least one neighbour
- [ ] Factor identified and contacted
- [ ] Renovation quoted before bidding, not after

---

# Appendix C — v0.2 Design Principles

Replacing v0.1's single principle with three, because the single one was ambiguous about the archive.

1. **Capture immediately, interpret later.** History is the only irreversible asset.
2. **Admit a dataset only when a named decision depends on it.** Maintenance is forever; curiosity is free only while parked.
3. **Prefer a reason over a score.** A number that hides its reasoning cannot be argued with, and this project's entire value is in being argued with.

> The measure of v0.3 is not that it contains more. It is that it contains less, and that what remains has survived contact with a real purchase.
