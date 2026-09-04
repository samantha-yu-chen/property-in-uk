# UK Property Intelligence System
## Implementation Plan — v0.3

**Document type:** Implementation plan (first version that commits to scope, schema and a build order)
**Status:** Draft — open for review by two reviewers before any build work starts
**Primary scope:** Edinburgh, Scotland — owner-occupier acquisition — **houses only, built in the last 10 years**
**Relationship to v0.1 / v0.2:** those are thinking documents (why); this is the implementation document (what gets built, in what order). Do not re-litigate the rationale here — it is settled in v0.2 §1–§6 and §14. This document only narrows further and adds build detail.
**End goal:** a personal decision-support app (start local, end iPhone) that a small number of named people (currently two) use to decide whether to bid on a specific Edinburgh house, how much, and to keep a written record of every property considered and why it was rejected.

---

# 1. Intent and End Goal, Restated

## 1.1 The question this system answers

> Given an Edinburgh house built in the last 10 years, currently for sale, should I bid on it, how much, and what specific reason would make me refuse to bid at any price?

Not "where is the best area in Edinburgh." Not "what is the market going to do." One property at a time, evaluated fast, with a written reason attached to the outcome either way.

## 1.2 The end artefact

A small app, used by two named people, that for any candidate property shows:

1. A **kill-checklist verdict** — pass / escalate / hard-stop, each with the specific reason, not a score.
2. A **bid ceiling** — anchored on Home Report value (or EPC + build cost proxy where no Home Report exists) plus the current segment premium from ESPC, capped by a pre-committed maximum.
3. A **builder dossier summary** — what else this builder/developer has built in Edinburgh in the last 10 years, their financial standing, and any Home Report issues previously logged against their stock.
4. The **funnel history** — every property either of us has looked at, searchable, with the reason it was pursued or dropped.

**Build order:** a local tool first (Stage A, §8), an iPhone app last (Stage B, §8). The app is a UI on top of data and rules that must already exist and already be useful without it. Building the app before the data pipeline produces real decisions is building the wrong thing first.

## 1.3 What this is explicitly not

Unchanged from v0.2 §2 and §14: not an investment or alpha engine, not a scoring model, not a product, not scraped bulk market data, not England/Wales/NI, not commercial. New for v0.3: also not a full retrospective builder-quality database — see §5.3 for why that specific ambition is bounded.

---

# 2. Scope Narrowing From v0.2

v0.2 scoped to "Edinburgh, owner-occupier, any property type." v0.3 narrows one more time, based on the actual next purchase in view:

| Axis | v0.2 | v0.3 |
|---|---|---|
| Property type | Any (flats, tenements, houses) | **Houses only** (detached / semi-detached / terraced). Flats and tenements out of scope for the working set. |
| Build age | Unrestricted | **Built in the last 10 years** (≈2016 onward), including both first-sale new-build and second-hand resale of recent stock |
| Data admitted | ~15 sources (v0.2 §7.1, §8) | Narrower still — see §3 table below |

### 2.1 Why "house, ≤10 years" removes real complexity

v0.2's heaviest, least-tractable risk category was tenement common-repair liability (shared roof, stonework, stair, factor quality — v0.2 §2.2, §5) because it lives in no dataset and is structurally invisible to a buyer. A standalone recent-build house mostly does not have a shared-stair, shared-roof problem. It replaces that risk with a different, narrower one:

- **Estate factor risk** — many new-build estates have a private factor for roads, drainage and landscaping until the council adopts them, sometimes for years. This is smaller in scope than tenement common-repair, but not zero, and it is checkable (ask for the factor deed / management agreement, ask what "adoption" status the estate roads are at).
- **Builder-specific defect risk** — replaces "this specific stair's roof condition" with "this specific builder's known pattern of issues." This is the new §5 dossier work.

This is a genuine simplification, not a relabelling. Most of v0.2 Appendix B's "structural / cost" section (factor-for-a-tenement-stair items) becomes conditional-not-applicable for this property type, and gets replaced by the smaller builder/estate-factor section in §7.

---

# 3. The Hard Constraint: New-Build First Sale Has No Home Report

This governs everything else in this document, so it is stated once, plainly, and not re-derived per section.

> **Newly built housing that has never been previously occupied is exempt from the Home Report requirement in Scotland.** Only once such a property is resold for the first time does a Home Report exist for it. ([e.surv](https://www.esurv.co.uk/products-and-services/are-home-reports-required-in-scotland/), [DM Hall](https://www.dmhall.co.uk/news-insights/exemption-reporting-hr/))

This splits the working set into two sub-types with genuinely different available data:

| | **Sub-type A: first-sale new-build** (buying direct from developer / off-plan) | **Sub-type B: recent resale** (built ≤10yr, being sold second-hand) |
|---|---|---|
| Home Report | **Does not exist.** | Exists — same as any other resale. |
| Primary condition evidence | EPC + NHBC/structural warranty + snagging inspection you commission yourself | Home Report (surveyor valuation + condition ratings 1/2/3) |
| Primary risk-screening lever | **Builder dossier** (§5) — because there is no independent surveyor opinion on this specific unit yet | Home Report + builder dossier, both |
| Pricing mechanism | Developer's list price, sometimes with incentives, not offers-over/closing date | Standard Scottish offers-over + closing date (v0.2 §4.1) |

Every candidate property must be classified A or B before anything else runs, because the downstream checklist and bid logic differ. This classification is the first field the funnel schema captures (§6).

---

# 4. Narrowed Data Source List

Replacing v0.2 §7.1's ~15 sources. Columns: kept as-is, demoted (kept in parking lot, not in the working pipeline), or dropped for this scope.

| Source | v0.2 role | v0.3 status | Reason |
|---|---|---|---|
| Home Report (per property) | Anchor for every bid | **Kept** — sub-type B only | Still the single best free per-property evidence, where it exists |
| Scottish EPC register | £/m² denominator | **Kept**, now the primary condition proxy for sub-type A | Bulk quarterly, filterable by build era — this is what makes "last 10 years" queryable at all |
| ESPC monthly House Price Report | Competition thermometer | **Kept** | Unchanged rationale, v0.2 §4.2 |
| Registers of Scotland free statistics | Liquidity proxy | **Kept**, also used as trading-history proxy for a specific development's postcode (§5.2) | Free, monthly, small-area |
| Scottish Assessors (SAA) council tax band | Ongoing cost | **Kept** | Static, cheap to join |
| Edinburgh Council planning portal + building warrant records | What's being built nearby | **Kept**, expanded role: also the source for "what has this builder built here" (§5.1) | Same source, new use |
| Companies House | — (not in v0.2) | **Added** | Builder financial health, phoenixing pattern — see §5.1 |
| Edinburgh Council school catchments | Resale demand at exit | **Kept** | Unchanged |
| SEPA flood maps | Insurability | **Kept** | Unchanged |
| OS Open UPRN | Joins | **Kept** | Unchanged |
| Historic Environment Scotland (listed/conservation) | Retrofit restriction | **Demoted** | Recent-build houses are very rarely listed or in conservation areas; check only if a specific candidate is an infill exception |
| Edinburgh Council STL licence register | Block character | **Dropped for this scope** | Tenement/flat-specific signal, not relevant to standalone recent-build houses |
| SIMD | Crime/deprivation proxy | **Demoted to parking lot** | Multi-year cadence, coarse, no named decision currently depends on it for this narrower property type — re-admit only if a specific candidate raises the question |
| statistics.gov.scot / NRS | Demand context | **Demoted to parking lot** | Slow-moving, not decision-critical for a single-property go/no-go |
| Ofcom Connected Nations | Liveability baseline | **Demoted to parking lot** | Nice-to-have, not blocking |

Net: **11 active sources**, down from v0.2's 15, with Companies House added as the one genuinely new source this scope requires.

---

# 5. The Builder Dossier

## 5.1 What it is now (buildable immediately, one-time + periodic)

For each active Edinburgh-area developer (a low-cardinality list — realistically 15–25 names: Barratt, Taylor Wimpey, Persimmon, Cala, Springfield, Dundas Estates, Miller Homes, Cruden, Avant, and similar), a static dossier record:

```
builder_name · parent_company (Companies House number)
developments_last_10yr[]  (name, area, postcode, approx unit count, completion year)
financial_snapshot        (Companies House filing status, any dissolution/reformation pattern)
factor_arrangement_notes  (who factors the estate roads/amenity, adoption status if known)
last_reviewed_date
```

**Sources:** council planning portal / building warrant records (which developer built which site), Companies House free API (financial filings, "phoenixing" — a company dissolving and a near-identical one reappearing is a known industry pattern worth flagging, not proof of wrongdoing on its own), the developer's own published site list.

This is manual, low-volume research, done once per builder and refreshed periodically (e.g. every 6 months) — not a scraping pipeline. It fits directly under the v0.2 Appendix C admission rule: a named decision (do I trust this developer enough to buy off-plan, or discount their resale stock) changes if this value changes.

## 5.2 Trading-history proxy per development

Because a new-build estate usually sits on its own tight postcode cluster, the Registers of Scotland free small-area statistics (§4) can be read at that postcode as a rough proxy for "how is this specific development trading" — transaction volume and price trend over time. This is **approximate, not builder-attributed** — if a Data Zone contains more than one developer's stock, the proxy blurs. State this limitation next to the number every time it is shown, not in a footnote.

## 5.3 What it is not, and cannot be made to be quickly

The original ambition — "pull every Home Report ever flagged against this builder's stock over the last 10 years" — is **not buildable as a retrospective bulk pipeline**, for the same reason v0.2 §2.1/§7.3 already ruled out portal scraping and paid RoS transaction data:

- ESPC does not retain Home Reports after a listing closes — there is no historical archive to query.
- Reconstructing one would mean either bulk-scraping ESPC across every historical resale in a development (prohibited — same wall as v0.2 §7.3), or buying RoS transaction-level data to at least get the trading history, which still wouldn't include historical Home Report content (declined — v0.2 §2.1, £675–933+VAT/month).

**Resolution: this layer is not retrospective, it is prospective, and it rides on the funnel.** Every property either reviewer looks at — pursued or not — gets tagged with `builder` and `development_name` in the funnel record (§6). The builder-level "what issues keep coming up in this developer's Home Reports" view is a rollup query over the funnel table, and it starts sparse and gets more useful every month, by design (v0.2 §6: capture now, interpret later). State this plainly in the app itself — the builder dossier's financial/site data is available from day one; its condition-history view is not, and will visibly say "N observations logged" so it's never mistaken for a complete picture.

---

# 6. Funnel Schema (extends v0.2 §10.1B)

```
date_seen · reviewer (Samantha | mate) · address · postcode · data_zone
property_subtype (A: first-sale new-build | B: recent resale)
builder · development_name
bedrooms · epc_floor_area · build_year
home_report_value (null if subtype A) · asking_price
date_listed · date_under_offer · went_to_closing_date
estate_factor_known? · estate_roads_adopted?
council_tax_band
home_report_condition_flags[]  (free text, tagged by category: roof / damp / drainage / other — null if subtype A)
subjective_score · reason_not_pursued
kill_checklist_result  (pass | escalate | hard-stop, + which rule fired)
```

Two additions over v0.2: `reviewer` (now two people log entries) and `property_subtype` / `builder` / `development_name` (drives everything in §5). `home_report_condition_flags` is what §5.3's builder rollup actually reads.

---

# 7. Kill Checklist v2 (extends Appendix B of v0.2)

Structural changes from v0.2's Appendix B:

- Tenement-specific items (factor-for-a-stair, roof/stonework-with-no-recorded-works) are marked **N/A for subtype A/B house purchases** — kept in the document for completeness (a candidate could still turn out to be an ex-tenement conversion) but not part of the default active list.
- **New section: Builder / Developer**
  - [ ] Builder has no dossier entry yet → escalate: build one before bidding, do not skip
  - [ ] Companies House shows a dissolution-and-reformation pattern for the developer entity → hard-stop pending explanation
  - [ ] Estate roads/amenity not yet adopted by council and factor arrangement unclear → escalate, get the factor deed
  - [ ] Funnel already has ≥3 logged Home Report condition flags in the same category (e.g. damp) across this builder's other stock → escalate, ask the seller's solicitor directly
- **Subtype A specific**
  - [ ] No independent snagging inspection commissioned before completion → hard-stop, this is the sub-type's substitute for a Home Report
  - [ ] Developer incentive (deposit contribution, stamp duty paid) not reflected as a price reduction in the bid ceiling calculation → recalculate before offering

Everything else in v0.2 Appendix B (regulatory, liquidity, price, human checks) carries over unchanged.

---

# 8. Build Order

**Stage A — local tool, both reviewers, no app store**

1. **Archive capture.** Dumb, timestamped, unparsed pulls of: Scottish EPC register (filtered to Edinburgh + build year ≥2016), ESPC monthly report, RoS free small-area stats. Flat files in dated folders. No schema decisions required — this can start the same day this document is approved.
2. **SQLite schema + funnel entry.** A single local `.sqlite` file implementing §6's schema. Entry via a small script or form (CLI is enough at this stage) — usable by both reviewers. SQLite chosen because it is a single file (git-friendly for two people, or shareable via a synced folder), needs no server, and is trivially replaceable later — consistent with v0.2 §10.2's "no code that takes more than an afternoon to replace."
3. **Builder dossier v1.** Manual research pass covering the 15–25 active Edinburgh developers (§5.1), stored as rows in the same SQLite file.
4. **Kill-checklist view.** A read-only query/report that, given a candidate property's funnel row, evaluates §7's rules and prints pass/escalate/hard-stop with reasons. Not a scoring model — a rule evaluator.

**Stage B — iPhone app, only after Stage A has real data in it**

5. A thin SwiftUI client reading from the same store (either the SQLite file synced between the two reviewers' devices, or a minimal personal backend if concurrent multi-device write turns out to matter in practice). Scope: same four views as Stage A (funnel log, builder dossier, kill-checklist verdict, bid ceiling) — no new logic, no new data sources. Building the UI is the last step, not an early one, because the rules and schema above are still going to move once real properties are run through them.

**Do not start Stage B until Stage A has processed at least a handful of real candidate properties.** An app around an untested schema is an app that gets rebuilt.

---

# 9. Open Questions for Review

Two reviewers now, so these are flagged explicitly rather than assumed:

1. **Joint vs solo buyer assumptions.** v0.1/v0.2's open questions (budget, LTV, stress-tested payment) were framed for a solo buyer. Does a second named person on the funnel imply joint purchase, joint finance, or is this collaboration on research only? This changes §1's LTV/ceiling math and should be settled before Stage A's kill-checklist ceiling logic is finalised.
2. **Shared storage mechanism.** A single SQLite file works for one person. With two reviewers logging entries, do we accept "one person edits at a time, sync via git or a shared drive" for Stage A, or is concurrent write access needed sooner than Stage B?
3. **Weekly time budget for builder-dossier maintenance and funnel entry**, split across two people — v0.2 §12.2 already names "too much time on maintenance" as a stopping condition; worth a number now that there are two contributors.
4. **Reviewer access to Companies House / paid searches**, if any dossier entry ever needs a paid document (e.g. a full company filing) — decide the ceiling for occasional small spend here, separate from the standing "no paid bulk data" position in §2.1/§7.2 of v0.2, which this document does not change.

---

# 10. What v0.3 Does Not Change

For the avoidance of doubt, everything in v0.2 not explicitly revised above still holds: the pricing-mechanism analysis (§4), the irreversibility-of-the-archive argument (§6), the declined paid data sources (§7.2), the prohibition on portal scraping (§7.3), the stopping conditions (§12.2), and the working thesis (§14). v0.3 is a narrowing and a build order, not a reversal.
