# UK Property Intelligence System
## Intent & Thinking Plan — v0.1

**Document type:** Pre-build thinking artefact / project constitution  
**Status:** Open discussion  
**Research snapshot:** 25 August 2026  
**Scope:** United Kingdom property intelligence, with UK nations treated as distinct legal/data jurisdictions where necessary  
**Stage:** Before 0 → 1  
**Not yet:** Implementation plan, build order, technical stack decision, model specification, investment rulebook, or production architecture

---

# 0. Why This Document Exists

This document is intended to capture the full ambition and current thinking behind a long-run UK property intelligence system **before deciding what should actually be built first**.

The purpose is not to reduce the idea into an MVP too early.

The purpose is to make the intent explicit enough that it can be challenged.

The system is imagined as a long-term personal investment research capability that combines:

1. a broad **Data Ball** around property,
2. a systematic search for **Market Bugs**,
3. dynamic understanding of **Law / Tax**,
4. a reusable **Common Property Kernel**,
5. a point-in-time **Time Machine**,
6. and a **Liquidity + Leverage** view of capital.

The central idea is:

> A property should not be analysed as an isolated house or flat.  
> It should be analysed as a node inside a changing economic, social, regulatory, geographic, infrastructure and capital system.

The long-run ambition is to discover repeatable, lawful market edges created by fragmented information, delayed repricing, poor synthesis, local market structure, regulation, liquidity, optionality or capital structure.

This is deliberately broader than a house-price analytics project.

---

# 1. Core Intent

The eventual goal is to build a property-focused intelligence system capable of bringing together as much useful UK market information as reasonably available and preserving the historical state of that information.

The system should eventually help answer questions such as:

- What is happening around a property that the headline asking price does not show?
- What has changed in an area before the property market fully reflects it?
- What appears attractive on gross yield but becomes unattractive after tax, regulation, voids, finance and exit friction?
- Which areas are liquid and resilient rather than merely expensive or fast-growing?
- Where is demand structurally increasing or decreasing?
- Which properties have optionality that is not fully priced?
- Which locations are improving or deteriorating underneath slow-moving headline statistics?
- Which market participants may be acting using incomplete information?
- Which signals repeatedly appear before price, rent, liquidity or demand changes?
- How much leverage can a position safely support under adverse conditions?
- When is waiting better than buying?
- When is an apparently good property actually a capital trap?
- When does a regulatory or tax change alter the economics of one ownership or operating strategy relative to another?

The system is intended to improve **decision quality**, not merely produce more information.

---

# 2. The Big Picture

Conceptually, the system can be thought of as six interacting pillars.

```text
                                  UK PROPERTY INTELLIGENCE

                                             │
                                             ▼
                                      PROPERTY / AREA
                                             │
           ┌─────────────────────────────────┼─────────────────────────────────┐
           │                                 │                                 │
           ▼                                 ▼                                 ▼
      1. DATA BALL                     2. MARKET BUG                     3. LAW / TAX
  What can we know?                Where may the market                What rules change
                                  be mispricing reality?               the economics?
           │                                 │                                 │
           └──────────────────────┬──────────┴───────────────┬─────────────────┘
                                  │                          │
                                  ▼                          ▼
                         4. COMMON PROPERTY KERNEL      5. TIME MACHINE
                         What is the common language?   What was knowable then?
                                  │                          │
                                  └────────────┬─────────────┘
                                               ▼
                                     6. LIQUIDITY + LEVERAGE
                                  Can the capital survive and exit?
                                               │
                                               ▼
                                      INVESTMENT UNDERSTANDING
```

These are not independent modules.

They constrain each other.

For example:

- Data without a Time Machine may create false backtests.
- Market Bugs without Law / Tax may be economically meaningless.
- Law without a Common Property Kernel cannot be applied consistently.
- Leverage without liquidity analysis can create apparent returns that are not survivable.
- A Data Ball without market hypotheses can become a giant collection exercise.
- A Market Bug without evidence can become storytelling.

---

# 3. Pillar One — The Data Ball

## 3.1 The idea

The Data Ball is the broad information universe surrounding a property, street, neighbourhood, town, city or wider market.

The ambition is intentionally large.

Instead of asking only:

> What did this house sell for?

the system should eventually be able to ask:

> What economic, demographic, physical, regulatory, infrastructure, behavioural and market conditions existed around this property at a particular point in time?

A property might therefore be surrounded by hundreds or thousands of observations.

```text
                                PROPERTY DATA BALL

                                      PROPERTY
                                          │
        ┌─────────────┬─────────────┬─────┼─────┬──────────────┬──────────────┐
        │             │             │           │              │              │
        ▼             ▼             ▼           ▼              ▼              ▼
      PRICE         RENTAL       PEOPLE      ECONOMY       PHYSICAL      REGULATION
        │             │             │           │              │              │
    sales history   long let      age          jobs          EPC            tax
    asking price    STR           health       income        flood          tenancy
    turnover        hotel         household    business      energy         planning
    discount        occupancy     tenure       births/deaths roads          licensing
    volatility      ADR           education    factories     water          restrictions
        │             │             │           │              │              │
        └─────────────┴─────────────┴───────────┴──────────────┴──────────────┘
                                          │
                                          ▼
                                  LOCATION / ACCESS
                                          │
                          schools / transport / shops
                          hospitals / parks / traffic
                          broadband / street lights
                          pollution / employment nodes
                          future infrastructure
```

The Data Ball should remain conceptually open.

A dataset should not have to prove its usefulness before entering the research universe.

However, entering the Data Ball does **not** mean it automatically becomes an investment signal.

---

## 3.2 Property and transaction data

Potential information includes:

- historical sale price
- transaction date
- registration date
- property type
- new-build flag
- tenure where available
- freehold / leasehold context
- cash vs mortgage market statistics
- transaction volume
- local transaction velocity
- repeat-sale history
- price-per-square-metre where floor area can be resolved
- sales distribution
- lower / upper quartile
- price band
- nearby comparable transactions
- new-build premium
- auction transactions where obtainable
- repossessions where obtainable
- corporate ownership
- overseas ownership
- title information
- land boundaries
- restrictive covenants where available
- lease-related information where licensing permits

### Strong free/open starting sources

**England & Wales**
- HM Land Registry Price Paid Data
- HM Land Registry UK House Price Index
- HM Land Registry INSPIRE Index Polygons
- selected HM Land Registry public ownership datasets

Price Paid Data contains residential sales lodged for registration from 1995 onward. It is updated monthly, but recent months are incomplete and can later be revised.

**Scotland**
- Registers of Scotland monthly house-price statistics
- Registers of Scotland small-area statistics
- UK House Price Index Scotland
- Registers of Scotland annual Property Market Report

A major difference is that detailed Scotland transaction-level sales products are available from Registers of Scotland under paid licences, while substantial aggregated statistics are available free.

**Northern Ireland**
- UK House Price Index provides UK-wide aggregate coverage.
- More detailed Northern Ireland property data should remain a specific data-landscape research question rather than being assumed equivalent to England/Wales.

### Important thought

The asymmetry between jurisdictions may itself matter.

The Data Ball should not pretend that “UK data” has a uniform structure.

---

## 3.3 Property identity and geography

Potential information includes:

- UPRN
- coordinates
- postcode
- street
- USRN
- building / land feature identifiers
- Output Area
- LSOA / MSOA
- Scottish Data Zone / Intermediate Zone
- ward
- local authority
- planning authority
- health geography
- parliamentary geography
- police geography
- travel-to-work area
- urban / rural classification
- catchment area
- custom radius / walking area / drive-time area

### Strong free/open starting sources

- Ordnance Survey OS Open UPRN
- OS Open USRN
- OS Open Linked Identifiers
- OS Code-Point Open
- OS Open Roads
- OS Open Greenspace
- OS Open Rivers
- OS Open Names
- ONS / Open Geography postcode and statistical geography lookups

OS Open UPRN provides persistent UPRN identifiers and coordinates for approximately 40 million addressable locations across Great Britain and is updated every six weeks.

### Core thinking

A postcode is not a property.

An LSOA is not a neighbourhood.

A council boundary is not a housing market.

The system will eventually need to support multiple spatial views simultaneously rather than force everything into a single geography.

---

## 3.4 Building and physical property information

Potential information includes:

- EPC rating
- historical EPCs
- floor area
- property age band
- construction type
- wall construction
- roof construction
- glazing
- heating system
- fuel type
- estimated energy consumption
- retrofit recommendations
- estimated environmental impact
- number of rooms where available
- building footprint
- plot characteristics
- terrain / elevation
- nearby water
- flood risk
- subsidence / geology
- radon
- coastal exposure
- heat risk
- climate exposure
- noise
- air quality

### Strong free/open starting sources

- Energy Performance of Buildings open data for England and Wales
- separate Scottish EPC sources
- Environment Agency flood datasets for England
- equivalent devolved environmental agencies for Scotland, Wales and Northern Ireland
- Ordnance Survey open terrain, roads, rivers and greenspace
- British Geological Survey open/public datasets where licensing permits
- national and local air-quality datasets

The England/Wales EPC service provides bulk CSV and developer API access and contains certificates registered since 2012.

---

## 3.5 Demographic information

Potential information includes:

- population
- population growth
- age distribution
- household composition
- family composition
- births
- deaths
- migration
- domestic migration
- international migration
- student population
- working-age population
- retired population
- household size
- owner occupancy
- social renting
- private renting
- vacant homes
- overcrowding
- education level
- occupation
- commuting mode
- car ownership
- ethnicity where relevant to aggregate demographic research and used responsibly
- language
- long-term population projections

### Strong free/open starting sources

- ONS Census
- ONS population estimates
- ONS Open Geography
- NOMIS labour-market datasets
- National Records of Scotland
- Welsh and Northern Irish statistical services
- local authority demographic releases

ONS Census data can reach small statistical areas and includes age, tenure, general health, occupation, qualifications and household characteristics.

### Important thought

Static demographics are less interesting than **demographic change**.

Possible future signals may come from:

```text
population mix at t0
        │
        ▼
population mix at t1
        │
        ▼
direction + rate of change
        │
        ▼
possible demand / affordability / amenity effect
```

---

## 3.6 Health and social environment

Potential information includes:

- self-reported health
- disability
- life expectancy
- avoidable mortality
- GP availability
- hospital distance
- emergency care accessibility
- mental-health statistics at appropriate aggregate level
- obesity statistics
- air pollution
- noise
- deprivation
- social isolation proxies
- local service capacity
- care-home concentration

### Potential free/open sources

- ONS Census health measures
- NHS open datasets
- Office for Health Improvement and Disparities datasets
- Scottish Public Health Observatory
- Public Health Wales
- Northern Ireland health statistics
- English Indices of Deprivation
- Scottish Index of Multiple Deprivation
- Welsh Index of Multiple Deprivation
- Northern Ireland Multiple Deprivation Measure

The English Indices of Deprivation 2025 provide LSOA-level measures spanning income, employment, education, health, crime, barriers to housing/services and living environment.

---

## 3.7 Crime and safety

Potential information includes:

- total crime
- crime type
- crime trend
- anti-social behaviour
- burglary
- vehicle crime
- violent crime
- bicycle theft
- public order
- outcomes
- crime concentration
- night-time crime
- British Transport Police incidents
- police-station accessibility
- perceived safety proxies
- street lighting as a possible environmental proxy

### Strong free/open starting sources

- data.police.uk API
- data.police.uk bulk downloads
- devolved / local police statistics
- local authority community-safety data

Important limitation:

Street-level police crime locations are anonymised/approximated rather than exact incident locations.

Another major jurisdiction issue:

The main Police.uk street-crime interface does not provide normal Scottish territorial-police coverage; Scotland requires separate sources.

That is exactly why jurisdiction-specific adapters will eventually matter.

---

## 3.8 Education

Potential information includes:

- school locations
- school type
- capacity
- pupil numbers
- admission pressure
- school performance
- exam results
- attendance
- absence
- Ofsted inspection history
- inspection outcomes
- free-school-meal share
- pupil demographics
- destinations
- school openings / closures
- school finance
- university proximity
- further-education access
- actual catchment where available
- inferred catchment where only admissions outcomes can be sourced

### Strong free/open starting sources

**England**
- Get Information About Schools
- Compare School Performance
- Explore Education Statistics
- Ofsted inspection datasets

Equivalent education sources exist separately for Scotland, Wales and Northern Ireland.

School establishment data can be updated daily; performance datasets are available historically.

### Possible market mechanism

A school is not automatically valuable because it has a high score.

Potential questions include:

- Is a school improving?
- Is demand exceeding capacity?
- Is the housing premium already fully priced?
- Do boundary changes create temporary repricing?
- Does school quality matter differently by property size and household type?

---

## 3.9 Economy, labour and businesses

Potential information includes:

- employment rate
- unemployment rate
- occupation
- sector composition
- wages
- income
- claimant count
- business registrations
- business closures
- company incorporations
- company dissolutions
- company accounts
- charges
- insolvencies
- local business density
- retail vacancy
- office vacancy
- industrial activity
- logistics activity
- major employers
- factory openings / closures
- university activity
- tourism economy
- inward investment
- commercial planning
- high-street change
- footfall where available

### Strong free/open starting sources

- ONS
- NOMIS
- Companies House API
- UK Business Counts
- local authority open data
- planning data
- insolvency statistics
- business-rates related open datasets where available

Companies House provides live public company information through a REST API, subject to API authentication and usage conditions.

### Important thought

“Number of businesses” is probably less interesting than:

```text
business formation
- business death
+ sector quality
+ employment intensity
+ physical footprint
+ persistence
+ local wage effect
```

---

## 3.10 Planning, land use and future development

This may be one of the most important Data Ball families because property markets can price the present more efficiently than they price the future.

Potential information includes:

- planning applications
- planning decisions
- application type
- granted / refused / withdrawn
- new housing
- major developments
- commercial developments
- conversions
- HMOs where planning-relevant
- change of use
- demolition
- infrastructure projects
- brownfield sites
- local plans
- supplementary plans
- land allocations
- design codes
- conservation areas
- listed buildings
- Article 4 directions
- green belt
- tree-preservation orders
- developer agreements
- infrastructure contributions
- flood zones
- protected land
- local-plan timetable
- planning-policy change

### Strong free/open starting source for England

The official Planning Data platform currently exposes more than 100 planning and housing datasets through a consistent interface, including planning applications, conservation areas, listed buildings, brownfield land, Article 4 areas, local plans and other planning constraints.

Important caveat:

Coverage and maturity vary by dataset and local authority, and the service is still evolving.

Scotland, Wales and Northern Ireland require different planning-data routes.

### Potential value

Planning data may allow a Time Machine to observe the sequence:

```text
proposal
   ↓
consultation
   ↓
application
   ↓
approval
   ↓
construction
   ↓
operation
   ↓
market repricing
```

Different information becomes public at each stage.

That timing may matter more than the final infrastructure itself.

---

## 3.11 Transport and accessibility

Potential information includes:

- railway stations
- service frequency
- journey time
- bus stops
- bus frequency
- road network
- traffic flow
- congestion
- traffic count
- vehicle type
- cycle infrastructure
- walking access
- airport access
- major-road access
- road noise
- future transport projects
- station openings
- line improvements
- closures
- travel-to-work time
- transport reliability

### Strong free/open starting sources

- Department for Transport road traffic API and bulk downloads
- National Public Transport Access Nodes
- NaPTAN / transport datasets
- Network Rail open data
- local transport authorities
- local authority transport planning
- OS Open Roads

The DfT road-traffic API currently provides Great Britain road-traffic estimates, count points, AADF, raw counts and local-authority statistics without API authentication.

Important limitation:

Small-area / individual-road-link estimates may be less robust than national and regional estimates.

---

## 3.12 Digital infrastructure

Potential information includes:

- broadband availability
- full fibre
- gigabit coverage
- broadband speed
- mobile coverage
- 4G
- 5G
- planned network deployment
- broadband take-up
- network competition

### Strong free/open starting source

- Ofcom Connected Nations datasets

Ofcom publishes current and historical information on fixed broadband and mobile coverage and provides downloadable supporting data.

### Potential mechanism

Digital infrastructure may matter differently for:

- remote workers
- rural properties
- high-income professional renters
- HMOs
- student housing
- holiday lets
- locations undergoing connectivity upgrades

---

## 3.13 Rental market

Potential information includes:

- asking rent
- achieved rent where obtainable
- rent distribution
- bedrooms
- property type
- furnishing
- days on market
- listing churn
- listing reductions
- supply
- demand proxies
- rental turnover
- rent-to-income
- rent per square metre
- private-rental stock
- social-rental stock
- voids
- tenancy duration where inferable
- letting-agent concentration
- landlord licensing

### Free/open source families

- ONS private-rental statistics
- government rental-price statistics
- local housing-market assessments
- Census tenure data
- local landlord registers/licensing where public
- VOA datasets where available
- portal data only where terms/licensing permit

### Major unresolved question

Open UK rental data is generally weaker than open sale-price data.

This is likely to be an important future gap.

It may ultimately require a mixture of:

- official aggregates,
- licensed/commercial feeds,
- legally collected listing history,
- and derived estimates.

---

## 3.14 Short-term rental / Airbnb

Potential information includes:

- listing supply
- active listings
- nightly price
- calendar availability
- minimum stay
- occupancy proxy
- reviews
- host concentration
- entire-home share
- neighbourhood
- seasonality
- listing creation
- listing disappearance
- regulatory exposure

### Free/open starting source

Inside Airbnb currently provides free quarterly data for UK locations including:

- Edinburgh
- London
- Bristol
- Greater Manchester

and also provides United Kingdom archive resources for research.

The available files include listings, calendars, reviews and neighbourhood geography.

### UK-level macro source

VisitBritain publishes monthly UK short-term rental market intelligence including supply and performance metrics.

### Important thought

Airbnb availability is not the same thing as realised occupancy.

Calendar blocking may represent bookings, owner use or unavailable dates.

STR datasets therefore require methodological caution.

---

## 3.15 Hotels and tourism

Potential information includes:

- hotel room prices
- hotel occupancy
- ADR
- RevPAR
- hotel supply
- new hotel openings
- tourism volume
- domestic tourism
- international tourism
- seasonality
- local events
- festivals
- conference demand
- airport passenger volume
- visitor attraction numbers

### Potential free/open sources

- VisitBritain / VisitEngland
- national tourism statistics
- local destination-management organisations
- airport statistics
- local event calendars
- planning applications for hotel supply

Hotel prices at a property-level/time-series level may require licensed sources or carefully permitted collection later.

---

## 3.16 Utilities, energy and water

The original idea explicitly includes electricity and water.

Potential information includes:

- electricity demand
- gas demand
- energy-consumption estimates
- network constraints
- grid connection capacity
- substation proximity
- renewable generation
- EV chargers
- smart-meter penetration
- power interruptions
- water consumption
- water stress
- sewer capacity
- water quality
- flooding from drainage
- planned infrastructure upgrades
- utility works

### Potential source families

- Department for Energy Security and Net Zero open energy data
- National Energy System Operator
- regional distribution-network operators
- Ofgem
- water companies
- Environment Agency
- local authority infrastructure data

### Current position

Do not assume utility datasets can all be resolved cleanly to a property.

Some may only exist at:

- network area,
- postcode,
- statistical area,
- feeder,
- local authority,
- or regional level.

That does not make them useless.

It means the Data Ball must preserve **native granularity**.

---

## 3.17 Street lights and hyper-local public realm

The original idea explicitly includes road-light counts.

Possible data includes:

- street lights
- lamp locations
- pavement quality
- cycle racks
- public bins
- trees
- parking spaces
- parking restrictions
- EV charging
- benches
- bus shelters
- CCTV where publicly documented
- street works
- road defects
- traffic-calming
- pedestrian crossings
- road accidents
- public realm investment

### Potential sources

Many of these are local-authority datasets rather than national datasets.

Coverage will therefore be inconsistent.

### Important principle

Street-light count should not be assumed to mean “safe”.

It might instead proxy:

- urban density,
- road intensity,
- local infrastructure investment,
- data completeness,
- pedestrian activity,
- neighbourhood design.

The system should preserve the observation first and test the market mechanism later.

---

## 3.18 Amenities and convenience

Potential information includes distance / access to:

- supermarket
- convenience store
- pharmacy
- GP
- hospital
- school
- nursery
- university
- railway station
- bus stop
- airport
- gym
- restaurant
- pub
- café
- park
- sports facilities
- library
- high street
- shopping centre
- major employer
- co-working space
- tourist attraction

Potential derived ideas:

- 5-minute walking environment
- 10-minute walking environment
- 15-minute city score
- public-transport accessibility
- car dependence
- amenity diversity
- amenity churn
- opening / closing trends

OpenStreetMap may eventually be important here, subject to its licence and attribution requirements.

---

## 3.19 Environmental and industrial context

Potential information includes:

- factories
- industrial estates
- waste sites
- landfill
- hazardous sites
- air-quality monitoring
- noise
- major roads
- rail lines
- airport flight paths
- contaminated land
- mines
- quarrying
- coastal erosion
- flood
- subsidence
- radon
- protected landscapes
- green space
- tree canopy
- water bodies

This information can represent both:

- downside risk,
- and future redevelopment optionality.

---

## 3.20 Data Ball architecture — conceptual, not final

At this stage the architecture should be understood conceptually rather than as a fixed technical implementation.

```text
                          EXTERNAL INFORMATION UNIVERSE

       official        local govt        market        spatial       regulatory
        data              data            data          data            data
          │                 │              │             │               │
          └─────────────────┴──────────────┴─────────────┴───────────────┘
                                           │
                                           ▼
                                 RAW OBSERVATION LAYER
                              preserve source + original form
                                           │
                                           ▼
                                  IDENTITY / GEOGRAPHY
                             property / place / area / network
                                           │
                                           ▼
                               COMMON PROPERTY KERNEL
                            common concepts and relationships
                                           │
                                           ▼
                                    TIME MACHINE
                             what existed / what was known
                                           │
                                           ▼
                              RESEARCH / MARKET-BUG LAYER
                              hypothesis + derived evidence
                                           │
                                           ▼
                              LIQUIDITY / CAPITAL CONTEXT
                                           │
                                           ▼
                                INVESTMENT UNDERSTANDING
```

This diagram does **not** imply a chosen database, cloud platform or engineering architecture.

---

# 4. Pillar Two — Market Bug

## 4.1 The idea

“Market Bug” is the working name for a persistent or recurring situation where market prices, rents, liquidity or behaviour may not fully reflect available reality.

A bug is not necessarily an error.

It may arise because of:

- fragmentation,
- timing,
- transaction friction,
- behavioural bias,
- institutional rules,
- information processing cost,
- local complexity,
- financing constraints,
- regulation,
- poor data,
- different investor objectives,
- or slow repricing.

The aim is to search for edges that are:

- lawful,
- observable,
- testable,
- economically meaningful,
- and potentially repeatable.

---

## 4.2 Initial Market Bug taxonomy

### A. Information Gap

The information exists but is fragmented across systems.

Example concept:

```text
planning + demographics + rent + crime + business change
                           │
                           ▼
             market participants rarely combine them
```

Possible edge:

Better synthesis.

---

### B. Timing Gap

Information becomes public before its full economic impact is priced.

Potential examples:

- infrastructure approval
- local-plan adoption
- school improvement
- employer expansion
- employer exit
- university expansion
- planning pipeline
- regulatory change
- new transport service
- regeneration funding
- broadband rollout

The important question is not simply:

> Did X happen?

It is:

> When did X become knowable, and when did the market react?

---

### C. Knowledge Gap

The facts are visible, but interpretation is difficult.

Examples:

- gross yield versus net yield
- leasehold economics
- building maintenance
- tax treatment
- planning restrictions
- licensing
- refurbishment economics
- insurance risk
- mortgage conditions
- short-let regulation

Potential edge:

Correct economic interpretation.

---

### D. Granularity Gap

Market discussion is often too coarse.

Examples:

- “Edinburgh”
- “Manchester”
- “EH3”
- “this council”

Actual markets may differ by:

- street,
- building,
- school catchment,
- side of a road,
- transport isochrone,
- micro-neighbourhood,
- property type,
- buyer segment.

Potential edge:

Analyse at the level at which behaviour actually differs.

---

### E. Liquidity Gap

Two properties with similar expected appreciation may have radically different exit characteristics.

Possible causes:

- narrow buyer pool
- unusual property type
- lease complications
- price-band discontinuities
- student concentration
- local transaction volume
- mortgageability
- seasonality
- local employment shock

Potential edge:

The market may underprice or overprice exit risk.

---

### F. Regulatory Repricing Gap

A rule changes economics before all owners/buyers fully adjust.

Examples might include changes in:

- transaction taxes
- landlord rules
- licensing
- planning
- short-term rental regulation
- energy requirements
- mortgage regulation

This should never mean exploiting illegal non-compliance.

The question is:

> Does a lawful rule change change relative property economics faster than the market reprices?

---

### G. Capital Structure Gap

The same property may have very different value to:

- a cash buyer,
- a leveraged owner,
- an individual landlord,
- a company,
- an owner-occupier,
- an investor with cheap capital,
- an investor with expensive capital.

Potential edge:

Price may be set by the marginal buyer, while another capital structure can produce different economics.

---

### H. Operational Gap

A slow process can itself produce opportunity.

Examples:

- due diligence takes other buyers days
- local regulation is difficult to understand
- comparables are manually assembled
- planning history is difficult to search
- rental economics are inconsistently calculated

Potential edge:

Faster and more complete decision-making.

---

### I. Optionality Gap

A property can have more than one economic future.

Potential options:

- owner occupation
- long-term rental
- short-term rental
- student rental
- HMO
- refurbishment
- extension
- subdivision
- conversion
- redevelopment
- change of use
- land value
- future sale to another buyer segment

The value of the property may therefore be:

```text
current use value
      +
future options
      -
cost / probability / regulation
```

---

### J. Behaviour Gap

Potential future research could examine:

- anchoring to asking price
- round-number price thresholds
- fear after local negative news
- overreaction to recent price falls
- underreaction to structural improvements
- seasonality
- auction dynamics
- new-build marketing premiums
- investor herd behaviour

This is an open research area, not an assumed source of alpha.

---

## 4.3 A Market Bug should eventually have a hypothesis structure

Not a fixed implementation requirement yet, but conceptually every serious market-bug idea should become something like:

```text
Observation
    │
    ▼
Possible mechanism
    │
    ▼
Who may be missing / mispricing it?
    │
    ▼
When does information become available?
    │
    ▼
What should happen if the hypothesis is true?
    │
    ▼
What evidence would disprove it?
    │
    ▼
Does the effect survive tax + cost + liquidity + leverage?
```

This protects the project from turning every interesting correlation into an investment story.

---

# 5. Pillar Three — Law / Tax

## 5.1 The idea

Law and tax should eventually be treated as dynamic market data, not static documentation.

Property economics change according to:

- jurisdiction,
- property type,
- ownership structure,
- intended use,
- buyer status,
- date,
- transaction structure,
- financing,
- tenancy,
- planning status,
- licensing,
- and regulation.

The same physical property can therefore have multiple economic realities.

---

## 5.2 UK jurisdiction is fundamental

The system must not treat the United Kingdom as a single legal property market.

At minimum the conceptual structure is:

```text
                       UK PROPERTY CONCEPTS
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
    ENGLAND                SCOTLAND                WALES
       │                      │                      │
       └──────────────────────┼──────────────────────┘
                              │
                      NORTHERN IRELAND
```

Different areas can have different:

- transaction taxes
- tenancy law
- landlord registration
- rent regulation
- planning systems
- short-term-let requirements
- property-register structures
- EPC systems
- licensing
- data availability

The Common Property Kernel should allow common comparison without pretending those differences do not exist.

---

## 5.3 Law / Tax information universe

Potential areas include:

### Acquisition
- SDLT
- LBTT
- LTT
- Northern Ireland transaction treatment
- additional-dwelling surcharges
- first-time buyer treatment
- non-resident treatment
- corporate treatment

### Ownership
- income tax
- corporation tax
- finance-cost treatment
- allowable expenses
- capital allowances where relevant
- ownership via individual / company / partnership structures
- council tax
- business rates
- annual charges where applicable

### Rental
- landlord registration
- tenancy rules
- rent rules
- deposit rules
- eviction / possession rules
- property standards
- HMO rules
- selective / additional licensing
- electrical / gas requirements
- EPC requirements
- right-to-rent rules where applicable

### Short-term rental
- planning
- registration
- licensing
- local restrictions
- maximum-night rules where applicable
- tax treatment
- business-rates interaction
- tourism levies if applicable

### Development / change
- permitted development
- planning permission
- conservation
- listed buildings
- Article 4
- building regulations
- developer contributions
- change of use
- local plans

### Disposal
- capital gains
- corporation tax on gains
- disposal costs
- timing
- residency
- inheritance considerations where relevant

---

## 5.4 The key idea: rules also have time

Eventually, a legal rule should conceptually look like:

```text
RULE
│
├── jurisdiction
├── scope
├── effective_from
├── effective_to
├── announced_at
├── enacted_at
├── source
├── interpretation status
└── applicable property / owner / strategy context
```

This matters because an investor in 2022 could not react to a rule first announced in 2025.

Law / Tax therefore belongs inside the Time Machine.

---

## 5.5 No fixed rulebook yet

v0.1 deliberately does **not** define:

- the “best” ownership structure
- a fixed leverage limit
- a minimum yield
- a required tax treatment
- a preferred tenancy strategy
- a BTL rule
- a STR rule

Those are future conclusions.

At this stage the goal is only to recognise:

> Law and tax are inputs to property economics and can themselves create or remove market edges.

---

# 6. Pillar Four — Common Property Kernel

## 6.1 The idea

The Common Property Kernel is the common language that allows otherwise incompatible datasets to describe the same market.

It is not yet a final schema.

It is not yet a chosen database.

It is not yet a microservice design.

Its purpose is conceptual:

> What are the persistent entities and relationships that must exist if all this information is ever going to be connected?

---

## 6.2 Candidate kernel concepts

Potential entities include:

```text
Property
Building
Land
Title
Address
UPRN
Street
Postcode
Area
LocalAuthority
PlanningAuthority

Transaction
Listing
RentalListing
ShortTermRentalListing
HotelMarketObservation

PersonSegment
HouseholdSegment
PopulationObservation
EconomicObservation
Business
Employer

School
Hospital
Amenity
TransportNode
Road
InfrastructureProject
UtilityAsset

PlanningApplication
PlanningPolicy
PlanningConstraint
Licence
Regulation
TaxRule

Loan
InterestRate
FinancingScenario
OwnershipStructure

MarketObservation
Event
DataSource
DataRelease
Revision
```

These are candidate concepts only.

---

## 6.3 Property may need multiple identities

A difficult but important issue:

```text
postal address
      ≠
UPRN
      ≠
land title
      ≠
building
      ≠
flat
      ≠
planning site
      ≠
commercial unit
```

For example:

- one building may contain many UPRNs,
- one title may contain multiple physical areas,
- a property may have changed address,
- a house may be converted into flats,
- two flats may later be combined,
- a planning application may cover several properties,
- an EPC may not map perfectly to a transaction address.

Therefore the kernel should probably model relationships rather than assume a single perfect “property ID”.

---

## 6.4 Geography is part of the kernel

A property exists simultaneously inside many areas.

```text
Property
│
├── postcode
├── Output Area
├── LSOA / Data Zone
├── MSOA / Intermediate Zone
├── Ward
├── Local Authority
├── Planning Authority
├── school context
├── police geography
├── health geography
├── flood geometry
├── custom radius
└── transport catchment
```

The system should eventually be able to ask:

> Which geography is appropriate for this particular question?

rather than use postcode by default.

---

## 6.5 Relationships may be more valuable than columns

The kernel is potentially better understood as a graph of relationships:

```text
PROPERTY
│
├── SOLD_AT ───────────────► TRANSACTION
├── LOCATED_IN ────────────► AREA
├── NEAR ──────────────────► SCHOOL
├── CONNECTED_TO ──────────► TRANSPORT
├── EXPOSED_TO ────────────► FLOOD_RISK
├── CONSTRAINED_BY ────────► PLANNING_RULE
├── ELIGIBLE_FOR ──────────► STRATEGY
├── FINANCED_BY ───────────► LOAN
└── OBSERVED_BY ───────────► DATA_SOURCE
```

The exact technical representation remains open.

---

# 7. Pillar Five — Time Machine

## 7.1 The idea

The Time Machine may ultimately be one of the most important parts of the entire project.

Property data is constantly:

- published late,
- revised,
- corrected,
- backfilled,
- reclassified,
- replaced,
- or removed.

If historical analysis uses today’s corrected dataset without knowing what was actually available at the time, it can create false intelligence.

The Time Machine asks:

> What was true then?

and separately:

> What could I have known then?

---

## 7.2 Four different times may exist

A useful conceptual example:

```text
Property sold                 10 January
Transaction registered        02 February
Dataset published             20 February
System collected it           21 February
```

These are four different events.

Possible time dimensions include:

- `event_time`
- `effective_time`
- `registration_time`
- `published_time`
- `observed_time`
- `ingested_time`
- `revised_time`

The final design is not decided, but these concepts should not be collapsed prematurely.

---

## 7.3 Why it matters

HM Land Registry explicitly notes that recent Price Paid Data is incomplete and later releases add transactions and revisions.

Therefore:

```text
today's historical dataset
           ≠
what the investor knew historically
```

Without preserving historical releases, a backtest can accidentally know future information.

This is **look-ahead bias**.

---

## 7.4 The Time Machine is bigger than backtesting

It can eventually reconstruct:

### Property history
- previous sale
- EPC state
- planning history
- listing history
- rent history
- renovation signals

### Area history
- population
- crime
- schools
- businesses
- transport
- broadband
- planning
- deprivation
- amenity change

### Regulatory history
- law
- tax
- licensing
- planning rules
- STR rules

### Capital history
- Bank Rate
- mortgage rates
- credit conditions
- LTV products
- rental stress tests

Then the question becomes:

> If we stood on 1 June 2018, what would this area have looked like using only information genuinely available by 1 June 2018?

That is a very different research capability from a normal property database.

---

## 7.5 Possible conceptual states

```text
                NOW VIEW
                   │
                   ▼
          what we know today
                   │
    ┌──────────────┴──────────────┐
    ▼                             ▼
RECONSTRUCT                   REVISIONS
    │                             │
    ▼                             ▼
PAST KNOWLEDGE STATE        what changed later
```

This may allow future questions such as:

- When was a local change first detectable?
- How long did property prices take to react?
- Did rental markets react before sale prices?
- Was a planning event already priced before approval?
- Which indicators lead and which lag?
- Did the signal work in real time or only with revised data?

---

# 8. Pillar Six — Liquidity + Leverage

## 8.1 The idea

Property returns should not be viewed only through appreciation or rental yield.

A position can look attractive and still fail because:

- it cannot be sold,
- refinancing disappears,
- rates rise,
- rent falls,
- a large repair occurs,
- regulation changes,
- the property becomes unmortgageable,
- transaction costs absorb the edge,
- capital is locked for too long.

Therefore the project should eventually connect property intelligence to **capital survivability**.

---

## 8.2 Liquidity is a property feature

Potential measures include:

- local sales per month
- turnover rate
- number of comparable transactions
- transaction seasonality
- price-band liquidity
- property-type liquidity
- buyer-pool size
- mortgageability
- time between sales
- listing duration where available
- asking-to-achieved spread
- price reductions
- failed listings
- relistings
- auction liquidity
- sale-volume drawdown during stress
- recovery after market shocks

Possible concept:

```text
Expected Return
      │
      ├── high liquidity ──► easier exit
      │
      └── low liquidity ───► capital lock + price concession risk
```

Two otherwise similar assets may deserve different valuations because their exit distributions differ.

---

## 8.3 Leverage should amplify evidence, not compensate for weak economics

Potential capital inputs include:

- purchase price
- equity
- LTV
- mortgage rate
- fixed period
- reversion rate
- arrangement fee
- repayment structure
- interest-only / repayment
- refinancing assumptions
- lender criteria
- stress rates
- rent coverage
- cash buffer
- maintenance reserve

Potential stress dimensions include:

```text
rates ↑
rent ↓
void ↑
maintenance ↑
tax ↑
price ↓
liquidity ↓
refinance LTV ↓
regulation changes
```

The future question should not simply be:

> How much can I borrow?

It should be:

> Under what combinations of downside events does this capital structure stop being survivable?

---

## 8.4 Liquidity + leverage interaction

This is especially important.

```text
                 LEVERAGED PROPERTY
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
        cash-flow risk           refinancing risk
             │                       │
             └───────────┬───────────┘
                         ▼
                  forced-sale risk
                         │
                         ▼
                     LIQUIDITY
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
          easy exit             weak market
              │                     │
              ▼                     ▼
        manageable loss        price concession
                                     │
                                     ▼
                              leverage magnifies loss
```

This interaction is one reason liquidity belongs inside the core project rather than as a secondary metric.

---

# 9. The Six Pillars Together

A property research question might eventually flow conceptually like this:

```text
PROPERTY / AREA
      │
      ▼
DATA BALL
What is around it?
      │
      ▼
COMMON PROPERTY KERNEL
How do these observations relate?
      │
      ▼
TIME MACHINE
What was knowable at the relevant date?
      │
      ▼
MARKET BUG
Is there a plausible mispricing mechanism?
      │
      ▼
LAW / TAX
Does regulation change the economic result?
      │
      ▼
LIQUIDITY + LEVERAGE
Can the position survive and can capital exit?
      │
      ▼
INVESTMENT THESIS
```

Another equally valid direction is:

```text
MARKET BUG IDEA
      │
      ▼
What data would be required?
      │
      ▼
DATA BALL
      │
      ▼
Can those observations be reconstructed historically?
      │
      ▼
TIME MACHINE
      │
      ▼
Do law / tax / financing destroy the edge?
      │
      ▼
Is the idea still economically interesting?
```

The project should support both directions.

---

# 10. Free / Open Data Landscape — Initial v0.1 Map

This is not exhaustive.

It is a first evidence that the proposed information universe is realistic enough to continue exploring.

| Data family | Example free/open source | Geography | Typical level | Current note |
|---|---|---|---|---|
| Residential sales | HM Land Registry Price Paid Data | England & Wales | transaction/address | Free; monthly; recent periods revised |
| House prices | UK House Price Index | UK | national to local aggregates | Free; historical series |
| Scottish sales | Registers of Scotland house-price statistics | Scotland | local / small-area aggregates | Free statistics; detailed transaction products can be paid |
| Property IDs | OS Open UPRN | Great Britain | property/addressable location | Free; ~40m identifiers; 6-week updates |
| Streets | OS Open USRN | Great Britain | street | Open identifier |
| Postcodes | Code-Point Open / ONS lookups | UK/GB depending product | postcode | Useful geography bridge |
| Roads | OS Open Roads | Great Britain | road | Open |
| Greenspace | OS Open Greenspace | Great Britain | spatial feature | Open |
| Rivers / terrain | OS open products | Great Britain | spatial feature | Open |
| EPC | Energy Performance of Buildings | England & Wales | certificate/property | Bulk + API |
| EPC | Scottish EPC sources | Scotland | certificate/property | Separate system |
| Demographics | ONS Census | England & Wales | OA upward | Very rich small-area data |
| Labour market | NOMIS / ONS | UK | area | Employment, occupation, earnings etc. |
| Deprivation | English Indices of Deprivation 2025 | England | LSOA | Free downloadable data |
| Deprivation | SIMD | Scotland | Data Zone | Separate Scottish index |
| Crime | data.police.uk | mainly England & Wales + BTP | approximate street / LSOA | Bulk + API; Scotland needs separate source |
| Schools | DfE school datasets | England | school | Rich historical data |
| Inspections | Ofsted | England | school | Inspection datasets |
| Planning | Planning Data | England | geometry / site / authority | 100+ datasets; evolving coverage |
| Flood | Environment Agency | England | spatial / postcode | Multiple open datasets |
| Traffic | DfT road traffic | Great Britain | count point / area | API + bulk; small-area precision varies |
| Broadband/mobile | Ofcom Connected Nations | UK | premises/area depending release | Downloadable coverage data |
| Companies | Companies House | UK | company | REST API |
| STR | Inside Airbnb | selected UK markets + UK archives | listing/neighbourhood | Free research data |
| STR market | VisitBritain | UK | market/region | Monthly market intelligence |
| Tourism/hotels | VisitBritain / VisitEngland | UK/England | market/region | Occupancy and tourism statistics |
| Health | ONS / NHS / public-health bodies | UK by jurisdiction | small area upward | Large data universe |
| Air quality | national/local environment sources | UK by jurisdiction | sensor/area | Fragmented |
| Utilities | DESNZ / Ofgem / network operators | UK | network/area | Mixed granularity |
| Water | water companies / regulators / environment bodies | UK by jurisdiction | network/area | Fragmented |
| Street lights | local authorities | local | point/street | Highly inconsistent |
| Amenities | OpenStreetMap + official datasets | UK | point/geometry | Licence/attribution important |
| Local government | council open-data portals | local | mixed | Potentially very rich but inconsistent |

---

# 11. Data Tiers

“Free data first” does not mean “free data forever”.

A useful conceptual hierarchy is:

## Tier A — Official Open Data

Preferred starting evidence.

Examples:

- HMLR
- ONS
- OS OpenData
- DfT
- Planning Data
- Companies House
- Ofcom
- government education
- public environment datasets

Advantages:

- provenance
- historical releases
- relatively stable definitions
- strong legal reuse framework where OGL applies

---

## Tier B — Public but fragmented data

Examples:

- council open portals
- local planning systems
- street lights
- parking
- licensing
- public registers
- local consultations
- infrastructure plans

Potentially high edge because fragmentation creates processing cost.

---

## Tier C — Research/community open data

Examples:

- Inside Airbnb
- OpenStreetMap
- community-maintained geographic resources

Potentially valuable but assumptions and licence terms must be explicit.

---

## Tier D — Free-access but restricted-use data

Some datasets can be downloaded without payment but still contain licensing restrictions.

“Free to download” must never be treated as synonymous with “unrestricted reuse”.

HMLR Price Paid Data itself includes address-data licensing considerations.

---

## Tier E — Commercial / licensed data

Potential future categories:

- detailed Scottish transactions
- property portals
- asking-price history
- achieved rent
- mortgage-product history
- hotel pricing
- footfall
- card spending
- mobile-location data
- detailed ownership/title data
- high-resolution utility data

Paid data should only be introduced when the expected information value is understood.

---

# 12. Data Source Registry — Conceptual Requirement

Eventually every source should be understood through metadata such as:

```text
source_name
publisher
jurisdiction
domain
coverage
native_geography
time_coverage
update_frequency
publication_lag
revision_behaviour
access_method
licence
commercial_use
attribution_required
cost
known_bias
known_gaps
identity_keys
historical_snapshots_available
```

This is not an implementation schema yet.

It is simply a reminder that **data provenance is part of the intelligence**.

---

# 13. Important Analytical Risks

## 13.1 More data does not automatically create more edge

A large Data Ball can produce false confidence.

Every derived signal must eventually answer:

> What mechanism connects this observation to property economics?

---

## 13.2 Correlation is not causation

Example:

```text
more cafés
   +
higher prices
```

Possible interpretations:

- cafés caused desirability,
- affluent residents attracted cafés,
- both were caused by regeneration,
- planning density caused both,
- the effect is purely urban density.

Market Bug research must attempt to distinguish these.

---

## 13.3 Selection bias

Properties that transact are not the same as properties that do not transact.

Listings are not the same as completed transactions.

Airbnb listings are not the same as occupied nights.

EPCs disproportionately appear when regulatory triggers occur.

Planning data can be incomplete.

These differences matter.

---

## 13.4 Geographic mismatch

Different datasets may refer to:

- property
- postcode
- Output Area
- LSOA
- MSOA
- Data Zone
- council
- police area
- catchment
- road segment
- point
- polygon

Naively joining everything by postcode can create misleading results.

---

## 13.5 Temporal mismatch

Example:

```text
2021 Census
2025 crime
2026 house price
```

Combining them into a single “current area score” without recognising different dates may be misleading.

---

## 13.6 Revision bias

Official datasets often revise recent history.

This is one of the core reasons the Time Machine exists.

---

## 13.7 Survivorship bias

If the system only observes:

- currently active companies,
- currently open shops,
- currently listed Airbnbs,
- currently existing schools,

it can miss the closures that may be more informative than openings.

---

## 13.8 Data availability can itself be biased

Areas with good local open-data programmes may look “richer” in information than areas with poor publication.

Missingness should not automatically become a negative property signal.

---

## 13.9 Market participants have different utility functions

A home buyer may rationally pay more than an investor.

A cash buyer may rationally accept economics that do not work for a leveraged landlord.

A developer may value land optionality differently from an owner-occupier.

There may be no single “correct price”.

---

# 14. Lawful Market-Edge Boundary

The phrase “market bug” should mean lawful economic opportunity.

The project should search for:

- information-processing gaps,
- timing gaps,
- structural market frictions,
- regulatory repricing,
- complexity,
- optionality,
- liquidity differences,
- capital differences.

It should not depend on:

- insider information,
- misuse of personal data,
- prohibited scraping,
- evasion,
- unlawful tax behaviour,
- regulatory non-compliance,
- market manipulation,
- deceptive transactions.

The long-term advantage should come from **better synthesis and discipline**, not breaking rules.

---

# 15. Questions the Project Eventually Wants to Answer

## Property-level

- Is this property cheap or expensive relative to genuinely comparable properties?
- What is unusual about it?
- What future options does it contain?
- What hidden constraints does it contain?
- How liquid is this exact property type?
- Does its EPC / construction / location create future capex?
- Has it repeatedly changed hands?
- How has its neighbourhood changed between previous transactions?

---

## Area-level

- Which areas are improving before price growth becomes obvious?
- Which areas are deteriorating while headline prices remain strong?
- Where is supply structurally constrained?
- Where is housing construction likely to alter future supply?
- Where are businesses forming?
- Where are jobs disappearing?
- Where are young households moving?
- Where are schools improving?
- Where is infrastructure moving?
- Where are crime patterns improving?
- Where is rental demand increasing faster than supply?
- Which markets retain transaction volume under stress?

---

## Market-level

- Which leading indicators precede property repricing?
- Which indicators merely follow price?
- Where does rental repricing lead sales repricing?
- Where does sales volume turn before price?
- How do interest-rate shocks propagate geographically?
- Which property types lose liquidity first?
- Which markets recover first?

---

## Regulation-level

- Which rules change relative economics between locations?
- Which rules change relative economics between ownership structures?
- Which regulatory changes create forced sellers?
- Which create barriers to new supply?
- Which increase operational cost?
- Which create optionality for compliant owners?

---

## Capital-level

- Which assets remain cash-flow positive under stress?
- Which properties depend on continued refinancing?
- Where does leverage improve capital efficiency without creating unacceptable ruin risk?
- Which investments have attractive expected returns but unacceptable liquidity tails?
- How much cash reserve changes survivability?
- When does paying more for a more liquid asset create better total economics?

---

# 16. Important Concepts That Should Stay Open

v0.1 deliberately refuses to lock the following decisions.

### Geographic scope

Do we eventually analyse:

- all UK,
- one nation,
- one city,
- selected property markets,
- or UK-wide architecture with research concentrated locally?

Open.

---

### Property scope

Do we include:

- residential only,
- BTL,
- owner-occupier,
- holiday let,
- HMO,
- mixed use,
- land,
- development?

Open.

---

### Investment horizon

Do we care about:

- months,
- 1–3 years,
- 5–10 years,
- lifetime cash flow,
- opportunistic trading?

Open.

---

### Market edge

Which Market Bug families are most promising?

Open.

---

### Technical architecture

No decision yet on:

- cloud
- local
- warehouse
- lakehouse
- graph database
- PostGIS
- DuckDB
- Snowflake
- Databricks
- object storage
- orchestration
- APIs

Those are future implementation questions.

---

### Models

No decision yet on:

- regression
- causal inference
- hedonic models
- AVMs
- survival analysis
- spatial statistics
- graph models
- machine learning
- forecasting

First understand the research questions.

---

### Scoring

There is no fixed:

- “good area score”
- “buy score”
- “crime weight”
- “school weight”
- “yield threshold”

A universal weighted score could destroy information rather than create it.

---

# 17. The Project Should Avoid One Giant “Good Area Score”

A major conceptual warning:

```text
crime      7/10
school     8/10
transport  9/10
yield      6/10
----------------
AREA SCORE = 7.5
```

This is easy to build and potentially dangerous.

Different strategies care about different features.

For example:

```text
Family owner-occupier
      ≠
Student BTL
      ≠
Short-term rental
      ≠
Development
      ≠
Retirement property
```

The same area can be excellent for one strategy and terrible for another.

The Data Ball should preserve dimensions before collapsing them.

---

# 18. Possible Research View: Property as a Dynamic State

Instead of thinking:

```text
Property = fixed object
```

consider:

```text
Property at time t
=
physical asset
+ neighbourhood state
+ regulation
+ demand
+ supply
+ capital conditions
+ optionality
+ liquidity
```

Therefore:

```text
Property(t0) ≠ Property(t1)
```

even when the bricks have not changed.

This may be one of the most important conceptual foundations of the whole project.

---

# 19. Possible Research View: Area Momentum

The project may eventually care more about **direction** than absolute level.

For example:

```text
Area A
school = excellent
crime = low
income = high
price = already high
change = flat
```

versus:

```text
Area B
school = average but improving
crime = falling
business formation = rising
young population = rising
planning investment = rising
transport = improving
price = not yet repriced
```

The second area may be more interesting from a Market Bug perspective even if it looks worse in a static snapshot.

This is a hypothesis, not a conclusion.

---

# 20. Possible Research View: Market State Transitions

An area might move through states:

```text
NEGLECTED
   │
   ▼
EARLY CHANGE
   │
   ▼
VISIBLE REGENERATION
   │
   ▼
DEMAND ACCELERATION
   │
   ▼
PRICE REPRICING
   │
   ▼
MATURE / FULLY PRICED
```

Potential research question:

> Can the Data Ball identify transitions before traditional house-price statistics do?

Again, this is one possible direction, not yet a build requirement.

---

# 21. Possible Research View: Liquidity Cycle

A market may change in this order:

```text
buyer interest
    ↓
listing activity
    ↓
transaction volume
    ↓
days to sell
    ↓
discounting
    ↓
price
```

or some other order.

The project should eventually discover the actual sequence from data rather than assume it.

This could be especially useful around:

- rate shocks,
- recessions,
- regulatory change,
- local employer shocks,
- housing-supply shocks.

---

# 22. Possible Research View: The Capital Stack as Part of the Asset

Traditional analysis often treats mortgage financing as external to the property.

This project may instead treat the investable object as:

```text
PROPERTY
+
OWNERSHIP STRUCTURE
+
FINANCING
+
REGULATION
+
OPERATING STRATEGY
```

Therefore:

```text
same property
+ different capital stack
=
different investment
```

This is especially important if the long-run goal is to use leverage conservatively to improve capital efficiency.

---

# 23. Potential Long-Run Moat

If this becomes valuable, the moat may not be access to any one dataset.

Most core data is public.

The potential moat is more likely to be:

```text
historical snapshots
        +
entity resolution
        +
geographic relationships
        +
clean time alignment
        +
regulatory history
        +
tested market hypotheses
        +
decision history
        +
outcome history
```

Over time:

```text
public data
     ↓
private structured history
     ↓
tested knowledge
     ↓
better questions
     ↓
better decisions
     ↓
new outcome data
     └──────────────► learning loop
```

The compounding asset is therefore not just data.

It is **structured evidence + historical context + tested market knowledge**.

---

# 24. What v0.1 Is NOT Trying to Decide

This document intentionally does not answer:

- what database to use,
- which city to start with,
- what dataset to ingest first,
- what programming language to use,
- what cloud to use,
- whether to use a graph database,
- whether to build a UI,
- whether to build APIs,
- whether to use machine learning,
- whether to buy commercial data,
- which investment strategy is best,
- what leverage level is safe,
- what yield is acceptable,
- what scoring formula should exist,
- what the first MVP should contain.

Those questions belong after the intent is challenged and refined.

---

# 25. Questions to Resolve Before Moving from Intent to 0 → 1

The next discussion should challenge the thesis itself.

## Purpose

1. Is the true objective capital growth, cash flow, total return, downside protection, or capital efficiency?
2. Is this primarily a personal investment research system or potentially a reusable platform?
3. Is the aim to find individual properties, areas, market regimes, or all three?
4. What does “repeatable edge” mean in property where transactions are infrequent?

## Market Bug

5. Which types of market inefficiency are plausibly persistent enough to exploit?
6. Which gaps disappear once transaction cost is included?
7. Are we looking for better prediction, better valuation, faster diligence, better risk control, or all of them?
8. What evidence would convince us that no useful market bug exists?

## Data Ball

9. How broad should the data universe become before breadth creates more noise than information?
10. Which data is truly property-level versus only contextual?
11. Should missing data itself become information?
12. When should a commercial dataset be considered?

## Common Property Kernel

13. What exactly is a “property” across UPRN, title, building, flat and land?
14. Should the central abstraction be property, location, event or market observation?
15. Which relationships need to survive across all UK jurisdictions?

## Time Machine

16. How much historical reconstruction is necessary?
17. Do we need historical source files or only publication timestamps?
18. How should revisions and deleted records be represented?
19. What counts as information genuinely knowable by an investor on a given day?

## Law / Tax

20. Should legal information be represented as source text, structured rules, scenarios or all three?
21. How do we represent rules that are announced but not yet effective?
22. How should uncertain interpretation be represented?

## Liquidity + Leverage

23. What does “safe leverage” actually mean?
24. Is survival under stress more important than maximising expected IRR?
25. How should liquidity risk enter valuation?
26. How should we value the option to wait rather than buy?

## Scope

27. Is UK-wide data architecture important from day one even if research later starts in one city?
28. Does Scotland deserve independent treatment from the beginning because its transaction/legal systems differ?
29. Should owner-occupied property and investment property remain inside the same conceptual model?
30. Should short-term rental and hotel demand be part of the core thesis or an optional strategy layer?

These are not blockers.

They are the next intellectual work.

---

# 26. v0.1 Working Thesis

The project thesis, in its current open form, is:

> UK property markets contain a large amount of publicly observable information that is fragmented across geography, time, jurisdictions and institutions.
>
> A sufficiently well-structured property intelligence system may create an advantage by connecting those observations around a common property/location model, preserving what was knowable at each point in time, understanding the law and tax context in which the asset operates, and studying how market prices, rents and liquidity react.
>
> The objective is not to assume that an edge exists. The objective is to build the intellectual and data foundation needed to search for, reject, validate and eventually exploit lawful and repeatable Market Bugs.
>
> Property should be considered together with liquidity and capital structure. Leverage is not the source of the edge; it is a tool that can amplify a well-understood edge but can also destroy survivability when liquidity or cash flow fails.
>
> The eventual compounding advantage may come less from proprietary raw data and more from historical snapshots, clean entity relationships, point-in-time evidence, tested hypotheses, regulatory understanding and accumulated decision/outcome history.

---

# 27. One-Page Mental Model

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                      UK PROPERTY INTELLIGENCE SYSTEM                        │
│                                                                             │
│   Goal: understand property as a changing economic system and search for    │
│         lawful, repeatable market mispricing / structural opportunity.      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  DATA BALL                                                                  │
│  price • rent • Airbnb • hotels • people • jobs • health • crime • schools │
│  planning • roads • rail • broadband • flood • EPC • utilities • amenities │
│  businesses • factories • environment • public realm • local government    │
│                     │                                                       │
│                     ▼                                                       │
│  COMMON PROPERTY KERNEL                                                     │
│  property ↔ address ↔ UPRN ↔ title ↔ building ↔ area ↔ events ↔ rules      │
│                     │                                                       │
│                     ▼                                                       │
│  TIME MACHINE                                                               │
│  what happened? • when published? • when known? • what was later revised?  │
│                     │                                                       │
│                     ▼                                                       │
│  MARKET BUG                                                                 │
│  information • timing • knowledge • granularity • liquidity • regulation   │
│  capital structure • operations • optionality • behaviour                   │
│                     │                                                       │
│                     ▼                                                       │
│  LAW / TAX                                                                  │
│  jurisdiction • date • ownership • use • planning • tenancy • licensing    │
│                     │                                                       │
│                     ▼                                                       │
│  LIQUIDITY + LEVERAGE                                                       │
│  exit • turnover • mortgageability • cash flow • refinance • stress        │
│                     │                                                       │
│                     ▼                                                       │
│                         INVESTMENT UNDERSTANDING                            │
│                                                                             │
│  Not: "Which postcode has the highest score?"                               │
│  But: "What is changing, what is mispriced, can we prove it, and can the    │
│        capital survive long enough to capture it?"                          │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 28. Current Conclusion

There is enough openly available UK data to justify continuing this idea.

The problem is not primarily:

> Can data be found?

The deeper problems are:

- can different property identities be resolved,
- can different geographies be related correctly,
- can historical publication states be preserved,
- can jurisdiction differences be represented,
- can real mechanisms be separated from correlations,
- can transaction friction be incorporated,
- can liquidity be measured,
- and can apparently attractive economics survive leverage and stress?

Those problems are not reasons to reduce the ambition yet.

They are the reason this project could become materially more useful than a conventional property dashboard.

The appropriate next step after v0.1 is therefore **not implementation**.

It is to challenge the thesis, expand or remove concepts, decide which questions are actually worth answering, and only then convert the surviving thinking into a 0 → 1 research and system plan.

---

# Appendix A — Current Verified Data Notes (August 2026)

These notes are included to anchor the discussion in the current UK data landscape. They are not intended to be an exhaustive source catalogue.

### HM Land Registry
- Price Paid Data covers residential property sales in England and Wales sold for value and lodged for registration.
- Full history is available from 1995.
- Data is updated monthly.
- Recent periods are incomplete and later revised.
- Public datasets also include UK HPI and INSPIRE Index Polygons.
- Some address-related reuse rights require careful licence review.

### Registers of Scotland
- Free monthly house-price statistics include mean/median prices, volume and value.
- Small-area statistics are available for detailed Scottish geographies.
- Annual Property Market Reports provide long-run market analysis.
- Detailed sales-data products are available commercially under licence.

### Ordnance Survey
- OS Open UPRN is free and provides persistent identifiers plus coordinates for approximately 40 million addressable locations across Great Britain.
- Open UPRN is updated every six weeks.
- Open USRN and Open Linked Identifiers help connect properties, streets and topographic features.
- OS also publishes free roads, greenspace, rivers, names, postcode and terrain products.

### Energy Performance of Buildings
- England/Wales EPC data is available in bulk CSV form and through a developer API.
- Historical certificates are included.
- Scotland uses a separate EPC system.

### Planning Data
- England's Planning Data service currently exposes more than 100 planning and housing datasets through one interface.
- Data includes planning applications and many spatial constraints/policy datasets.
- Bulk formats include CSV, JSON, GeoJSON and in some cases Parquet.
- Coverage and platform maturity remain evolving.

### Police.uk
- Bulk and API crime data is available.
- Street-level locations are anonymised / approximate.
- Scottish territorial crime is not represented in the same way and requires separate sourcing.

### Department for Transport
- Great Britain road-traffic API includes count points, AADF and raw counts.
- Bulk downloads are also available.
- Fine-grained road estimates should be interpreted with quality limitations.

### Ofcom
- Connected Nations publishes current/historical UK broadband and mobile coverage.
- Supporting data downloads are available.
- Spring 2026 data reflects January 2026 infrastructure state.

### Companies House
- Public company information is available through a REST API.
- API credentials are required.
- Data can help examine business formation, company presence, charges, filings and closures.

### Education
- England provides establishment, performance, pupil and Ofsted datasets.
- Historical school-performance datasets are downloadable.
- Other UK nations operate separate education systems and sources.

### Deprivation
- English Indices of Deprivation 2025 provide LSOA-level ranks, scores, domains and underlying indicators.
- Scotland, Wales and Northern Ireland maintain separate deprivation frameworks.

### Inside Airbnb
- Free quarterly research datasets currently include Edinburgh, London, Bristol and Greater Manchester.
- UK archive resources are available.
- Listings, calendars and reviews can provide STR supply and pricing proxies.
- Availability should not automatically be interpreted as realised occupancy.

### VisitBritain
- Monthly UK short-term rental intelligence includes supply and performance metrics.
- Tourism and accommodation statistics provide additional context.

---

# Appendix B — Candidate Data Ball Domains Checklist

This list is intentionally broad.

- [ ] Property identity
- [ ] Address
- [ ] Title / tenure
- [ ] Transaction history
- [ ] Price
- [ ] Asking price
- [ ] Comparable sales
- [ ] Transaction volume
- [ ] Turnover
- [ ] Listing duration
- [ ] Rental price
- [ ] Rental supply
- [ ] Rental demand
- [ ] STR / Airbnb
- [ ] Hotel market
- [ ] Tourism
- [ ] EPC
- [ ] Floor area
- [ ] Building age
- [ ] Construction
- [ ] Energy
- [ ] Flood
- [ ] Climate
- [ ] Geology
- [ ] Subsidence
- [ ] Radon
- [ ] Air quality
- [ ] Noise
- [ ] Green space
- [ ] Population
- [ ] Age
- [ ] Household
- [ ] Tenure
- [ ] Migration
- [ ] Health
- [ ] Deprivation
- [ ] Employment
- [ ] Occupation
- [ ] Income
- [ ] Business
- [ ] Factory / industrial activity
- [ ] Insolvency
- [ ] Retail / high street
- [ ] School
- [ ] University
- [ ] Nursery
- [ ] Crime
- [ ] Police
- [ ] Road
- [ ] Traffic
- [ ] Rail
- [ ] Bus
- [ ] Airport
- [ ] Walking
- [ ] Cycling
- [ ] Parking
- [ ] EV charging
- [ ] Broadband
- [ ] Mobile connectivity
- [ ] Planning applications
- [ ] Planning approvals
- [ ] Local plan
- [ ] Brownfield
- [ ] Conservation
- [ ] Listed buildings
- [ ] Article 4
- [ ] Development pipeline
- [ ] Infrastructure pipeline
- [ ] Utility infrastructure
- [ ] Electricity
- [ ] Water
- [ ] Sewerage
- [ ] Street lights
- [ ] Street works
- [ ] Public realm
- [ ] Amenities
- [ ] Shops
- [ ] Restaurants
- [ ] Healthcare access
- [ ] Local government finance
- [ ] Council tax
- [ ] Business rates
- [ ] Property tax
- [ ] Landlord law
- [ ] Rental regulation
- [ ] STR regulation
- [ ] Licensing
- [ ] Financing conditions
- [ ] Mortgage rates
- [ ] Mortgage availability
- [ ] LTV
- [ ] Bank Rate
- [ ] Credit conditions
- [ ] Market liquidity
- [ ] Historical revisions
- [ ] Source provenance

The checklist is not a commitment to ingest everything.

It is the current boundary of the information universe we may want to investigate.

---

# Appendix C — v0.1 Design Principle

> **Preserve optionality in the thinking before optimising implementation.**

At this stage:

- collect questions before choosing models,
- understand information before choosing scores,
- understand time before backtesting,
- understand jurisdictions before normalising rules,
- understand liquidity before using leverage,
- and understand the possible Market Bug before deciding what data deserves to become a signal.

The project is still in the stage of deciding what system is worth building.
