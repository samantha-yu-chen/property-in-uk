# UK Property Intelligence System
## Implementation Plan — v0.4

**Document type:** Implementation plan (extends the v0.3 MVP scope, schema and build order)
**Status:** Draft — open for review by two reviewers before any build work starts
**Primary scope:** Edinburgh, Scotland — owner-occupier acquisition — **houses and purpose-built flats in buildings completed within the last 10 years**
**Relationship to v0.1 / v0.2:** those are thinking documents (why); this is the implementation document (what gets built, in what order). Do not re-litigate the rationale here — it is settled in v0.2 §1–§6 and §14. v0.3 is the implementation baseline; v0.4 preserves its workflow and adds conditional common-building diligence. Explicit revisions here take precedence over inherited assumptions.
**End goal:** a personal decision-support app (start local, end iPhone) that a small number of named people (currently two) use to decide whether to bid on a specific in-scope Edinburgh property, how much, and to keep a written record of every property considered and why it was rejected.

---

# 1. Intent and End Goal, Restated

## 1.1 The question this system answers

> Given an Edinburgh house or purpose-built flat in a building completed within the last 10 years, currently for sale, should I bid on it, how much, and what specific reason would make me refuse to bid at any price?

Not "where is the best area in Edinburgh." Not "what is the market going to do." One property at a time, evaluated fast, with a written reason attached to the outcome either way.

## 1.2 The end artefact

A small app, used by two named people, that for any candidate property shows:

1. A **kill-checklist verdict** — PASS / ESCALATE / HARD STOP, each with the specific reason, not a score.
2. A **system-calculated bid ceiling** — with an explainable, auditable calculation separating pricing anchor, market adjustment/context, confirmed quantifiable property costs/liabilities, calculated property ceiling, personal affordability/willingness-to-pay maximum and final bid ceiling (§6.1). Resale uses Home Report valuation and relevant ESPC context; first-sale uses available comparables and incentive-adjusted developer pricing. EPC/build-cost proxies are not independent valuations. Insufficient required pricing evidence → ESCALATE, not an invented ceiling.
3. A **builder dossier summary** — a reasonably complete, decision-useful baseline created when the builder/developer first appears in a real candidate, then reused and updated as justified. It covers known/discovered relevant Edinburgh developments, available financial evidence and prospective condition observations; it is not a complete retrospective catalogue or defect history.
4. The **funnel history** — every property either of us has looked at, searchable, with the reason it was pursued or dropped.

**Build order:** a local tool first (Stage A, §8), an iPhone app last (Stage B, §8). The app is a UI on top of data and rules that must already exist and already be useful without it. Building the app before the data pipeline produces real decisions is building the wrong thing first.

## 1.3 What this is explicitly not

Unchanged from v0.2 §2 and §14: not an investment or alpha engine, not a scoring model, not a product, not scraped bulk market data, not England/Wales/NI, not commercial. Retained from v0.3: also not a full retrospective builder-quality database — see §5.3 for why that specific ambition is bounded.

---

# 2. Scope Revision From v0.3

v0.2 scoped to Edinburgh owner-occupier acquisition across property types. v0.3 narrowed to recent houses. v0.4 admits recent purpose-built flats without reopening older-building scope.

| Axis | v0.3 | v0.4 |
|---|---|---|
| Property type | Houses only | Houses (detached / semi-detached / terraced) and purpose-built flats |
| Building age | Last 10 years | Building completed within the preceding 10 years, assessed at date considered |
| Sale subtype | First-sale / recent resale | First-sale / resale, independent of property type |
| Workflow | Archive, funnel, dossier, checklist | Same workflow plus manual candidate-building diligence |

**The age constraint applies to the building itself**, not unit creation, conversion, renovation or EPC issue date. Verify completion evidence for the candidate building, not merely the overall development. A build-era estimate is a discovery aid, not confirmed eligibility. Unknown age/type → ESCALATE; confirmed out-of-scope → HARD STOP for this version, with a scope reason rather than a claim that the property is unsafe.

Explicitly out of scope: buildings more than 10 years old, traditional/older tenements, converted flats in older buildings, and historic-building fabric and related retrospective complexity. Flats must be purpose-built. Use a rolling ten-year boundary at consideration date rather than a permanent 2016 cutoff. Where only a year is known near the boundary, retain uncertainty.

### 2.1 What simplification remains, and what is reversed

v0.2's heaviest risk category was tenement common-repair liability: shared roof, stonework, stair, drainage and factor quality, handled through records and human enquiry rather than an open dataset. v0.3 removed much of this from the default house workflow.

**Adding flats reverses this important v0.3 simplification: common-building liabilities return. Recent flats do not have the same risk profile as recent houses.** Modern apartment blocks can fall within Scotland's broader tenement definition; excluding traditional tenements does not remove shared maintenance responsibilities ([Scottish guidance](https://www.mygov.scot/tenement-repairs-common-areas)).

The recent-building boundary still avoids historic fabric, older conversions and long retrospective reconstruction. Houses retain builder and applicable estate-factor checks for roads, drainage, landscaping and adoption. Flats add conditional building checks (§7.4); houses with shared arrangements also receive applicable checks.

The main additional cost is **per-building evidence collection and review**, not major software or ingestion infrastructure. Checklist growth does not directly predict human diligence time. This remains a bounded MVP: no market-wide block database, paid bulk feed, bulk historic Home Report reconstruction or new application stage.

---

# 3. The Hard Constraint: First-Sale New-Build Normally Has No Home Report

New homes sold off-plan or to their first occupier are exempt from Scotland's Home Report requirement. Ordinary marketed resales normally require one. Record actual report availability and any exemption; a sale label alone does not prove whether a report exists ([official guidance](https://www.mygov.scot/buy-home/home-report)).

Classify `property_type` and `sale_subtype` independently before downstream rules run. Support all four: `house + first_sale`, `house + resale`, `flat + first_sale`, `flat + resale`.

| | **first_sale** (developer / off-plan) | **resale** (recent stock sold second-hand) |
|---|---|---|
| Home Report | Normally not required; record exemption and any report provided | Normally required; missing expected report → ESCALATE |
| Condition evidence | Independent inspection/snags, warranty documents and relevant completion evidence | Home Report survey/ratings plus targeted follow-up; visual survey is not complete defect history |
| Energy evidence | EPC for energy and floor area, not a condition survey | EPC / Home Report energy section, same limitation |
| Risk evidence | Builder dossier plus unit/development evidence; building diligence for flats | Home Report plus dossier and unit/development evidence; building diligence for flats |
| Pricing | Developer price and incentives, available comparables and personal maximum | Home Report and relevant market context; offers-over/closing date where applicable |

Off-plan first-sale candidates may be logged provisionally, but proposed completion is estimated evidence, not an already completed building. Keep eligibility ESCALATE until actual completion evidence establishes the scope requirement before completion of purchase; this does not admit an older-building conversion.

Unit snagging or a Home Report does not establish all common-element condition, safety or funding. An unexpired warranty is evidence to review, not proof every defect will be remedied.

---

# 4. Narrowed Data Source List

Preserve the v0.3 source shortlist, with conditional candidate-level additions. Parked sources remain outside the working pipeline unless a named decision requires them.

| Source | Original role | v0.4 status | Reason |
|---|---|---|---|
| Home Report (per property) | Anchor for every bid | **Kept** — normally resale | Still the single best free per-property evidence, where it exists |
| Scottish EPC register | £/m² denominator | **Kept** — energy and floor-area evidence | Build era can shortlist candidates; confirm building completion separately, not from certificate date |
| ESPC monthly House Price Report | Competition thermometer | **Kept** | Unchanged rationale, v0.2 §4.2 |
| Registers of Scotland free statistics | Liquidity proxy | **Kept**, with a development-context proxy only where published geography supports it (§5.2) | Free, monthly, small-area |
| Scottish Assessors (SAA) council tax band | Ongoing cost | **Kept** | Static, cheap to join |
| Edinburgh Council planning portal + building warrant records | What's being built nearby | **Kept**, expanded role: also the source for "what has this builder built here" (§5.1) | Same source, new use |
| Companies House | — (not in v0.2) | **Added** | Builder financial health, phoenixing pattern — see §5.1 |
| Edinburgh Council school catchments | Resale demand at exit | **Kept** | Unchanged |
| SEPA flood maps | Insurability | **Kept** | Unchanged |
| OS Open UPRN | Joins | **Kept** | Unchanged |
| Historic Environment Scotland (listed/conservation) | Retrofit restriction | **Conditional lookup** | Check applicable infill/site restrictions without admitting older fabric or historic conversions |
| Edinburgh Council STL licence register | Block character | **Conditional lookup for flats** | Block liveability where useful; no licence record does not prove absence of short letting |
| SIMD | Crime/deprivation proxy | **Demoted to parking lot** | Multi-year cadence, coarse, no named decision currently depends on it for this narrower property type — re-admit only if a specific candidate raises the question |
| statistics.gov.scot / NRS | Demand context | **Demoted to parking lot** | Slow-moving, not decision-critical for a single-property go/no-go |
| Ofcom Connected Nations | Liveability baseline | **Demoted to parking lot** | Nice-to-have, not blocking |

Additional candidate-level sources:

| Source | Use | Acquisition |
|---|---|---|
| Scottish Property Factor Register | Appointed factor registration; not a performance guarantee | Manual public [lookup](https://www.propertyfactorregister.gov.scot/) |
| Seller / developer / solicitor / factor documents | Services, title provisions, charges, works, liability allocation, insurance, funds and claims | Manual candidate requests, extending existing factor enquiries |
| Applicable safety assessments and lender responses | Relevant building-safety evidence, remediation status and lending requirements | Manual documents/enquiries where relevant |

**No new mandatory bulk ingestion pipelines.** EPC, ESPC and RoS remain the bulk archive. The factor register is one new public lookup; STL is conditionally re-admitted. Document requests are diligence inputs, not standing market feeds. Do not assume a complete public cladding evidence feed or universal statutory EWS1 requirement ([Scottish factsheet](https://www.gov.scot/publications/cladding-remediation-programme-factsheet/pages/mortgages-and-lending/)).

### 4.1 Evidence, uncertainty and provenance

**Unknown != Pass. Absence of evidence is not evidence of absence.** Retain source, observation/capture date, evidence type, status (`confirmed | estimated | unknown`), scope (unit/building/development/builder), document reference or note, and report date where available. Confirmed means supported by identified evidence, not guaranteed safety. Stale, conflicting or limited evidence remains explicit and may require ESCALATE. Confirmed N/A requires a reason; unknown values are not zero.

Initial screening rarely provides complete factor accounts/reserves/arrears, title-based repair shares and decision rules, planned works, insurance terms, warranty/common-defect claims, safety reports or shared-system records. Home Reports may disclose some but are not complete building evidence packs. First-sale proposed charges are estimates with no established operating history. Record missing evidence, next request, responsible reviewer and decision deadline; it may remain unavailable even after enquiry.

Do not turn unknown liabilities into arbitrary monetary allowances or composite scores. Capture only data with a plausible decision, diligence or historical-record role. Archive manual evidence with the same provenance as published sources.

**Legal / professional boundary:** the system records evidence, outstanding questions and professional conclusions; it does not independently interpret complex legal documents or replace solicitor, surveyor, lender, factor or other professional advice. It may record a solicitor-confirmed repair share, factor documents and professionally confirmed title-derived conclusions, or unknown liability allocation requiring review. It must not independently derive legal liability from complex title deeds. This is a recording boundary, not a new legal-analysis feature.

---

# 5. The Builder Dossier

## 5.1 Candidate-driven dossiers (on-demand + periodic refresh)

Stage A researches a builder/developer only when it first appears in a real candidate property. On that first encounter, create a **reasonably complete, decision-useful baseline dossier** using available public and candidate-relevant evidence: relevant entity/parent identity, known developments, financial/filing information, available factor/estate arrangements and existing candidate condition evidence, with sources and gaps explicit. This is more than a one-line identity record, but not a complete retrospective defect history.

The baseline may include other reasonably discoverable Edinburgh developments useful for understanding the builder, not only the current candidate's development. Neither every development from the last ten years nor full Edinburgh-wide builder coverage must be reconstructed before Stage A works.

On later encounters, **reuse the existing dossier**. Update only where new candidates/developments, changed financial information, new factor/estate arrangements, new condition observations or stale evidence justify it; do not rebuild from scratch for each candidate. Existing condition history grows prospectively. The baseline record is:

```
builder_name · parent_company (Companies House number)
known_developments[]     (known/discovered relevant Edinburgh sites, not an exhaustive catalogue; name, area, postcode, approximate unit count, completion year where known)
financial_snapshot        (Companies House filing status, any dissolution/reformation pattern)
factor_arrangement_notes  (who factors the estate roads/amenity, adoption status if known)
last_reviewed_date
```

**Sources:** council planning portal / building warrant records (which developer built which site), Companies House free API (financial filings, "phoenixing" — a company dissolving and a near-identical one reappearing is a known industry pattern worth flagging, not proof of wrongdoing on its own), the developer's own published site list.

This is manual, low-volume research: create the baseline on first encounter, reuse it thereafter and make justified updates. Lightweight periodic review (e.g. every 6 months) checks whether evidence is stale or changed; it does not require a full rebuild or a scraping pipeline. It fits the v0.2 Appendix C admission rule: a named decision about further diligence or whether to proceed changes if the evidence changes. Builder concerns alone do not justify an invented monetary discount.

## 5.2 Trading-history proxy per development

Read available RoS small-area statistics as rough market context only when the published geography plausibly represents a development. Do not promise postcode, building or type-level detail the source does not supply. Mixed developments/types blur the proxy. Show geography and limitations next to every number: **estimated context, not builder-attributed or a building valuation**. Missing granularity stays unknown.

## 5.3 What it is not, and cannot be made to be quickly

The ambition to pull every historic Home Report flagged against a builder is not a feasible complete retrospective bulk pipeline. The plans identify no complete accessible archive; portal scraping stays excluded, and paid transaction data would not supply report content.

**This is an accepted design constraint, not a Stage A blocker.** No decision-useful baseline builder/developer dossier at all may trigger ESCALATE until the evidence-backed baseline described in §5.1 exists. Incomplete retrospective history by itself must not keep a property in ESCALATE; sparse prospective history is acceptable and stays visibly labelled as incomplete. Other specific decision-critical gaps are assessed separately under the jointly agreed rules (§9.5). The condition-history layer is prospective and rides on the funnel. Link considered properties to builder/developer, development and building, and retain observation type, scope and provenance. Financial/site facts can be researched from day one but also retain missing information explicitly.

Display "N observations across M buildings/developments; incomplete prospective sample", including N = 0. "No known issues" means none recorded in this sample, not evidence of no issues. Multiple flats reporting one common defect are one building-level event, not independent builder-wide evidence. Remove the arbitrary ≥3 trigger; severity, relevance, independence and evidence quality guide escalation without statistical claims from small samples.

The dossier supports decisions and enquiry; it does not certify an individual property/development as safe. Record the actual builder and developer separately when different, including relevant entity identity rather than only parent branding. Relevant building/development-specific evidence takes precedence over general builder observations. An adverse observation is a reason to investigate applicability, not automatically invent a discount.

---

# 6. Funnel Schema (minimal extension of v0.3)

Use the same SQLite store, with small linked records populated only for considered candidates and relevant builders/developments/buildings. This is not a market-wide building database.

**Property/unit and funnel**

```
property_id · date_seen · reviewer · address · postcode · data_zone
property_type (house | flat) · sale_subtype (first_sale | resale)
builder_id · developer_id (may match builder_id) · development_id · building_id
bedrooms · epc_floor_area · asking_price · council_tax_band
home_report_value (nullable) · home_report_availability / exemption_note
date_listed · date_under_offer · went_to_closing_date
estate_factor_status · estate_roads_adoption_status
condition_observations[] (category, scope, evidence_id)
liveability_notes · reason_not_pursued
market_context_evidence_ids[] · pricing_evidence_ids[]
personal_maximum (nullable) · personal_maximum_evidence_id
bid_calculation_ids[] · current_bid_calculation_id (nullable)
eligibility_result · rule_results[] · kill_checklist_result
```

**Linked identities**

```
builder/developer: reusable §5 baseline dossier, known_developments[] and justified updates; entity identity / Companies House number where known
development: development_id · name · builder_id · developer_id · site_notes
building: building_id · development_id (nullable) · address/reference
          completion_date/year · completion_evidence_id · purpose_built_status
          management_notes · evidence_ids[]
```

Flats require a building link. Houses use the same minimal completion record; populate common-building diligence only where relevant. Development completion does not substitute for building completion in a phased site. Unknown identity links stay nullable with explicit unknown status and follow-up rather than guesses.

**Evidence and rule records**

```
evidence_id · subject_type/id · topic · source · observed/captured_at
report_date (nullable) · evidence_type · status (confirmed | estimated | unknown)
value/note · document_reference · limitations
rule_id · rule_version · applicable? (+ reason) · evidence_ids[]
result (PASS | ESCALATE | HARD STOP) · reason · next_action
responsible_reviewer · decision_deadline · joint_review/acceptance_note
```

Topics cover completion, condition, estate arrangements and §7.4. Store owner-specific liability allocation against the unit referencing building evidence; never assume equal shares. Monetary values require supporting evidence/status; unknown is not zero. Evidence and observations are not Home-Report-only, so first-sale inspections can be logged without pretending a report exists. An empty observation list is not evidence of no defects.

Independent `property_type` and `sale_subtype` replace the overloaded `property_subtype`. Evidence-backed statuses replace bare booleans where uncertainty matters. Liveability notes replace the former subjective score and do not drive automated scoring. Reuse records and checks across both types.

## 6.1 Explainable bid-ceiling calculation and audit trail

The **system calculates** the ceiling; it does not merely store a human-entered result. Reviewers provide/review evidence and the pre-committed personal maximum. Keep these components distinct:

1. **Market/pricing anchor:** Home Report valuation for resale, or a supported first-sale pricing anchor from comparables and incentive-adjusted developer pricing.
2. **Market adjustment/context:** record the relevant segment evidence and why any adjustment applies. Context does not automatically become a monetary premium.
3. **Confirmed quantifiable property costs/liabilities:** use only evidence-supported liabilities or costs that are reasonably quantifiable. A professional cost estimate may supply a labelled estimated amount for a confirmed issue; an uncertain risk cannot be monetised as an arbitrary discount.
4. **Calculated property ceiling:** the supported pricing anchor plus justified signed market adjustments, less justified cost/liability adjustments under the recorded calculation rule. Avoid double-counting matters already reflected in the anchor or incentives. Not every cost is a pound-for-pound price deduction.
5. **Personal affordability / willingness-to-pay maximum:** the separate pre-committed cap, with documented affordability assumptions. Recurring charges normally inform affordability; do not silently capitalise them into a discount without a supported rule.
6. **Final system bid ceiling:** `min(calculated_property_ceiling, personal_maximum)` when both are supported and available. Missing required inputs leave the relevant output unavailable and produce an outstanding request, not zero or a fabricated number.

Every monetary input/adjustment, including the personal cap and any intermediate incentive conversion, retains value, evidence reference/source, relevant date, rationale and calculation rule/version. A calculation step/command uses current evidence and the applicable rule version to write a new calculation record into SQLite. Preserve previous runs rather than overwrite them, so a later reviewer can reproduce why a number was produced. The read-only report/view displays the latest saved run and previous history without modifying historical calculation records: calculate → save calculation run → display. Non-quantifiable/unresolved risks remain in the checklist/escalation path: a calculated number does not clear a risk verdict or authorise an offer.

Minimal linked calculation records in the existing SQLite store:

```
calculation_id · property_id · calculated_at · rule_version · rule/formula_snapshot
inputs[] (component, value, units/period, evidence_id, relevant_date,
          evidence_status, rationale, applied_rule/version)
calculated_property_ceiling (nullable) · personal_maximum (nullable)
final_bid_ceiling (nullable) · explanation · unresolved_required_inputs[]
```

Evidence references resolve to existing §6 provenance records; no new identity model or ingestion system is needed. Both reviewers agree the applicable pricing/adjustment rules and sufficient input evidence before using results for real offers (§9.3 and §9.5), refining them on real Stage A candidates.

---

# 7. Kill Checklist v0.4

## 7.1 Verdict and eligibility

- **PASS:** applicable required checks have sufficient reviewed evidence and no unresolved decision-critical issue at the stated decision stage. Not proof of safety.
- **ESCALATE:** missing, uncertain, stale, conflicting or limited evidence needs targeted enquiry/professional review. Record what is needed, from whom and by when. Unknown decision-critical evidence normally escalates.
- **HARD STOP:** confirmed scope exclusion, breach of the pre-committed personal maximum, or a finding/unmet requirement classified as a stop under jointly agreed rules. Distinguish universal constraints from personal risk/affordability limits; record the specific reason.

Detailed criticality, evidence sufficiency, automatic versus personal stops, residual-uncertainty acceptance and decision-stage deadlines are deliberately left to both reviewers (§9.5). The categories below guide enquiry, not an exhaustive preset threshold table. Relevant non-critical unknowns may remain visible without blocking PASS under those agreed rules; genuinely decision-critical unknowns cannot be silently waived. If criticality itself is unsettled for an imminent decision, seek joint review rather than defaulting to PASS.

Aggregate HARD STOP first, then ESCALATE; PASS only after applicable requirements are satisfied. N/A needs an applicability reason. Initial screening can say "no adverse evidence found; diligence outstanding", but cannot present overall PASS while decision-critical evidence is unknown. Recent construction does not pass builder, factor, safety or communal liabilities.

Verify Edinburgh location, house/purpose-built-flat type and building completion within the preceding ten years (§2). Confirmed older/traditional tenement or older-building conversion → HARD STOP for scope; unknown eligibility → ESCALATE.

## 7.2 Common workflow retained from v0.2/v0.3

- EPC F/G, SEPA flood risk and adjacent planning affecting light/noise/outlook → review relevant impact, insurability and mortgageability evidence; ESCALATE where decision-critical under agreed rules.
- Low available transaction volume, unusual property type, price-band boundary or building/tenure lending constraint → review relevance and ESCALATE where required by agreed rules; preserve aggregate-data limitations.
- £/m² above relevant same-type comparables → review evidence and willingness-to-pay, not an automatic defect score. Inadequate pricing evidence → ESCALATE.
- Offer above the pre-committed personal maximum → HARD STOP. Final system bid ceiling unavailable or a proposed offer exceeding it → joint review before offering under agreed pricing/decision rules; do not invent missing inputs.
- Weekday-evening/weekend visits, neighbour discussion, applicable factor contact and renovation quotes → record findings/completion; outstanding decision-critical enquiries → ESCALATE.
- Applicable conservation/infill restrictions → targeted permitted-work/cost enquiry without admitting excluded older fabric.
- Estate adoption/management unclear → review deed/agreement and responsibilities; ESCALATE where decision-critical under agreed rules. Estate adoption does not establish building management adequacy.

Supported costs inform affordability and, where justified, the explicit calculation adjustments in §6.1; keep market context and personal willingness-to-pay separate. Unknown liability stays a diligence question, not an invented price adjustment. Confirmed risk or documented liability produces HARD STOP only under an applicable jointly agreed universal/personal rule.

## 7.3 Builder / developer and sale-specific checks

- No decision-useful baseline dossier → may trigger ESCALATE until the evidence-backed baseline in §5.1 exists at the agreed stage. Later candidates reuse it with justified updates rather than rebuild it. Incomplete retrospective history alone must not maintain ESCALATE; sparse prospective observations remain acceptable and explicitly labelled incomplete.
- Relevant dissolution/reformation or financial concerns → review entity identity, explanation and remedial responsibility; ESCALATE where decision-critical under agreed rules. Company structure alone is not proof of wrongdoing. Whether a confirmed inability to satisfy an obligation produces HARD STOP depends on the jointly agreed rule and relevant professional conclusion.
- Serious relevant defects or credible repeated observations → review unit/building applicability and independent follow-up; ESCALATE where decision-critical under agreed rules. No arbitrary count threshold or statistical claim; deduplicate shared-building events.
- `resale`: expected Home Report missing or decision-critical survey findings unresolved → ESCALATE for report/follow-up.
- `first_sale`: review independent inspection/snags arrangements and scope limitations. Both reviewers agree inspection requirements, decision deadline and whether absence is a personal HARD STOP; do not preset that threshold here. Unit snagging does not replace common-building evidence.
- `first_sale`: incentives omitted from pricing comparison → ESCALATE/recalculate before offering, including temporary charge contributions. Incentives do not eliminate later recurring costs.

## 7.4 Conditional common-building path for flats

All checks belong in **Stage A**, initially using available evidence and followed up manually. Apply relevant shared-arrangement checks to houses too. Unknown decision-critical evidence → ESCALATE; confirmed exposure is assessed against jointly agreed universal/personal HARD STOP rules.

| Check | Evidence / action |
|---|---|
| Factor / management and owner decisions | Identify manager, applicable registration, services/authority, relevant deeds and functioning owner decision process. No factor is an enquiry trigger, not proof repairs are impossible. |
| Common condition / repairs / planned works | Shared roof/structure, external walls/common fabric, stairs/communal areas and drainage; Category 3 common findings, disclosed defects or planned works need records, remedy and funding evidence. No historic roof replacement is not itself suspicious in a recent building. |
| Liability allocation / owner's share | Obtain unit-specific responsibility and repair share; unknown cannot mean zero or equal shares. |
| Recurring charges / reserves | Review charges, budget, reserves/sinking funds where applicable and planned major expenditure. First-sale charges are estimates, not operating history. Fund absence prompts assessment of documented funding, not automatic rejection. |
| Building insurance | Review applicable cover, excesses, exclusions and relevant issues; factoring charges do not prove adequate cover. |
| Building safety | Relevant fire/external-wall evidence, assessments, remediation responsibilities/status and lender requirements where applicable; no universal EWS1 rule. |
| Warranty / common-defect claims | Actual cover, expiry, exclusions, claims and remedial responsibility; warranty brand/remaining term is not a condition pass. |
| Shared systems | Applicable lifts, communal heating, drainage, access/parking and other shared systems: maintenance, known failures and replacement exposure. Reuse common-repair evidence rather than duplicate it. |
| Block liveability | Visits/neighbours on noise, security and occupancy; STL lookup where useful. Missing records do not prove absence of short letting or issues. |

Detailed accounts, arrears, title allocations, insurance, claims, safety and maintenance records are often unavailable at initial screening (§4.1). Request relevant evidence later within Stage A; do not assume PASS or defer diligence to Stage B. Not every missing document permanently blocks PASS: both reviewers agree criticality, sufficient resolution evidence and acceptable residual uncertainty at each decision stage (§9.5). Record any conscious acceptance, rationale, reviewer agreement and rule applied; evidence stays unknown where still unknown. A genuinely decision-critical unknown continues to prevent PASS at its required stage. Acceptance alone does not make evidence confirmed or bypass an applicable universal constraint.

---

# 8. Build Order

**Stage A — local tool, both reviewers, no app store**

1. **Archive capture.** Dumb, timestamped, unparsed pulls of: Scottish EPC register (Edinburgh + available recent build-era discovery filter; verify rolling completion eligibility separately), ESPC monthly report, RoS free small-area stats. Flat files in dated folders. No parsing required — start once approved. Retain source/capture dates and archive candidate documents as they arrive with §4.1 provenance.
2. **SQLite schema + funnel entry.** A single local `.sqlite` file implementing §6's schema. Entry via a small script or form (CLI is enough at this stage) — usable by both reviewers under an agreed single-writer/share arrangement (§9). Include linked candidate buildings, manual evidence entry and explicit unknowns. SQLite chosen because it is a single local file, needs no server, and is trivially replaceable later — consistent with v0.2 §10.2's "no code that takes more than an afternoon to replace."
3. **Builder dossier v1.** On a builder/developer's first appearance in a real candidate, create a reasonably complete baseline from available public and candidate-relevant evidence (§5.1), including other useful reasonably discoverable Edinburgh developments. Store it in the same SQLite file. Later encounters reuse the dossier and update only where new candidates/developments, changed financial information, factor/estate arrangements, condition observations or stale evidence justify refresh. Periodic maintenance remains lightweight. Full market/development coverage is not required; incomplete retrospective history and sparse prospective samples do not block Stage A.
4. **Bid-calculation command and read-only checklist/report.** Separate responsibilities within the same Stage A tool:
   - **Calculation step/command:** use current evidence and the applicable rule version to calculate the property ceiling and final system bid ceiling (§6.1). Write a new versioned calculation run into SQLite with inputs, evidence references, rationale and rule/formula snapshot; preserve previous runs rather than overwrite them. Update the current-calculation reference to the newly saved run.
   - **Read-only report/view:** evaluate §7's checklist rules and display PASS / ESCALATE / HARD STOP with reasons, evidence references and unresolved requests. Display the latest saved calculation and allow inspection of previous saved runs, including inputs, evidence, rule/version and rationale. The view does not itself calculate/save a new bid run or modify historical calculation records. Include conditional flat checks, distinguish screening from reviewed diligence, and keep market context, property risk and personal ceiling separate.

   **Flow:** calculate → save calculation run → display through the read-only view. These are responsibilities within Stage A, not new application stages or a scoring model.

All flat checks and manual building-evidence review are Stage A work. No separate flats stage or bulk building ingestion is introduced. Measure actual per-building review effort rather than infer time from checklist growth.

**Stage B — iPhone app, only after Stage A has real data in it**

5. A thin SwiftUI client reading from the same store (a controlled shared-store/export arrangement, or a minimal personal backend if concurrent multi-device writes turn out to matter in practice). Scope: same four views as Stage A (funnel log, builder dossier, kill-checklist verdict, bid ceiling) — no new logic, no new data sources. Building the UI is the last step, not an early one, because the rules and schema above are still going to move once real properties are run through them.

**Do not start Stage B until Stage A has processed at least a handful of real candidate properties, exercising house/flat diligence and first-sale/resale rules with available real evidence.** An app around an untested schema is an app that gets rebuilt.

---

# 9. Review Checklist and Genuinely Open Questions

## 9.1 Review of the agreed design

- [ ] Reviewers agree with **"Edinburgh houses and purpose-built flats in buildings completed within the last 10 years."** Building age excludes older tenements and older-building conversions; all four type/sale combinations are supported.
- [ ] Conditional common-building liabilities and per-building manual workload are understood; recent flats are not equivalent-risk houses.
- [ ] Unknown decision-critical evidence stays visible and normally ESCALATE; no score or arbitrary liability allowance substitutes for enquiry.
- [ ] Stage A includes flat diligence/provenance; Stage B remains the thin iPhone client.

**Accepted design constraint:** builder condition history is prospective and incomplete. Complete retrospective reconstruction is not required and is not an unresolved Stage A blocker. Review that coverage limits, deduplication and building-specific evidence precedence remain explicit.

## 9.2 A. Needed before / at the start of Stage A

Agree a temporary shared-storage/single-writer arrangement and how snapshots/evidence files are shared. If simultaneous entry is necessary, choose controlled writes rather than concurrent edits to a synced SQLite file. The questions below are not all blockers before coding; unresolved rules can remain explicit while capture, funnel entry and provisional calculations are developed.

## 9.3 B. Needed before real offers or relevant diligence deadlines

- Research collaboration versus joint purchase/finance; budget, deposit, LTV and stressed monthly payment.
- Pre-committed personal affordability/willingness-to-pay maximum and acceptable documented recurring charges/financial liabilities.
- Applicable pricing anchors, calculation/adjustment rules and evidence requirements for auditable system bid ceilings (§6.1).
- Personal liveability non-negotiables, risk/HARD STOP thresholds, evidence deadlines and responsibility for professional enquiries, coordinated with §9.5.

## 9.4 C. Learn and refine during Stage A

- Realistic weekly dossier-maintenance, funnel-entry and diligence workload across both reviewers.
- Per-candidate research/document/title-search, inspection and specialist-advice spending; use free Companies House material where sufficient. Agree an interim limit before incurring spending, then refine it from real cases; paid bulk feeds stay excluded.
- Actual effort required for flat/building evidence collection, including requests that remain unavailable. Checklist growth is not a time estimate.

## 9.5 D. Decision-rule questions to agree jointly

Both reviewers should discuss and agree these rules together, refining them using real Stage A candidate properties rather than inventing a complete threshold matrix in advance. Record the applicable rule/version and rationale for actual decisions.

- Which unknowns are decision-critical at each stage, and which relevant non-critical unknowns may remain visible without blocking PASS?
- What evidence, scope, freshness and professional conclusions are sufficient to clear each ESCALATE?
- Which confirmed findings should automatically HARD STOP under universal constraints, and which depend on personal tolerance, affordability or liveability limits?
- When may residual uncertainty be consciously accepted, by whom, and with what recorded rationale? Acceptance must not turn unknown evidence into confirmed evidence or override a genuinely decision-critical unknown at its required stage.
- At which stage must each check be resolved: initial screening, before an offer or binding commitment, during relevant diligence, or before purchase completion? How are deadlines and consequences applied?
- What evidence makes the first-encounter baseline dossier reasonably complete and decision-useful (§5.1), distinct from accepted sparse/incomplete condition history, and what independent inspection scope/deadline is required for first-sale candidates?

These are deliberately unresolved joint reviewer decisions, not a requirement to settle every threshold before Stage A coding. Before relying on a PASS, HARD STOP or calculated ceiling for a real action, agree the rules relevant to that candidate and decision stage.

---

# 10. What v0.4 Does Not Change

For the avoidance of doubt, everything in v0.2 not explicitly revised above still holds: the pricing-mechanism analysis (§4), the irreversibility-of-the-archive argument (§6), the declined paid data sources (§7.2), the prohibition on portal scraping (§7.3), the stopping conditions (§12.2), and the working thesis (§14). v0.4 retains archive, funnel, prospective builder dossier, local SQLite and Stage A → Stage B. Its explicit revisions are recent-flat inclusion, conditional common-building diligence, evidence-backed uncertainty and independent property/sale dimensions. Inherited universal Home Report or absent-common-risk assumptions are qualified by §§2–3 and §7. This is an MVP implementation plan, not a product redesign.
