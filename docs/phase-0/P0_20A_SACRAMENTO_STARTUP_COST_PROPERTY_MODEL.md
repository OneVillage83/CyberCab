# P0.20A — Sacramento Startup Cost & Property Model v0.1

**Project:** CyberCab Detail + Cab  
**Primary geography:** Sacramento, California  
**Backup geography:** Rancho Cordova / surrounding Sacramento industrial submarkets; West Sacramento only when property economics justify the utility/geography tradeoff  
**Status:** RESEARCH BASELINE / ACTIVE DESIGN  
**Documented:** 2026-09-04

## Purpose

Establish a current, evidence-based physical startup model for CyberCab Detail + Cab using actual September 2026 Sacramento-area industrial listings, current equipment prices, California car-wash registration requirements, Sacramento washwater rules, current commercial EV charging costs/incentives, and realistic working-capital allowances.

This document deliberately separates:

- **verified current listing/vendor price** — a published current asking price;
- **market benchmark** — a published comparable used for planning when a specific listing is broker-priced;
- **planning allowance** — our conservative budget range, not a vendor quote;
- **external quote required** — a cost that depends heavily on the exact property, insurance exposure, utility service, lease terms, or permitting.

The objective is not to find the biggest possible depot. It is to identify the **lowest-capital physical site that can legally and operationally teach us the right production lessons**.

---

# 1. Primary finding: a fenced all-concrete / paved yard can be enough to start

A conventional warehouse or permanent automatic car wash is **not** required for the first physical pilot.

A fenced concrete or asphalt industrial outdoor-storage yard can support the first operation if the property passes all of the following gates:

1. intended fleet storage / vehicle-service use is allowed by zoning and the landlord;
2. commercial vehicle washing/detailing is allowed at the site;
3. washwater can be legally captured and disposed of;
4. potable/process water is available or a lawful alternative is designed;
5. there is a sanitary-sewer disposal route, serviceable sump, or other approved disposal method for collected washwater;
6. electrical capacity is adequate for the initial charging plan;
7. the site has secure vehicle access and sufficient circulation;
8. insurance can cover the aggregate value of customer vehicles in our care, custody, and control;
9. charger and temporary/permanent wash equipment can be installed with landlord approval and required permits.

### Why paved/concrete is attractive

For our use case a paved yard is usually preferable to gravel because it provides:

- cleaner vehicles and lower dust load;
- easier inspection/computer-vision baselining;
- easier striping and position identification;
- more predictable circulation;
- better wet-vac / containment cleanup;
- easier charging-stall construction;
- less mud and track-out during rain;
- a cleaner environment for a future inspection portal.

### Important limitation

**Concrete does not itself make vehicle washing compliant.**

Sacramento County Environmental Management states that commercial vehicle washwater may not be discharged to the storm-drain system, waterways, streets, or off-site areas. The County explicitly identifies portable containment methods as compliance alternatives, including drain covers/barriers with wet-vac collection, portable boom capture, a drive-on wash pad with sump pump, certain truly low-volume DI cleaning, a permanent contained wash pad connected to sanitary sewer, and recirculating systems.

That creates a useful startup path:

```text
PAVED / CONCRETE YARD
        |
        v
PORTABLE DRIVE-ON WASH MAT
        |
        v
SUMP / WET-VAC CAPTURE
        |
        v
APPROVED SANITARY-SEWER / SERVICEABLE-SUMP DISPOSAL
```

A permanent plumbed wash bay can therefore be deferred until vehicle volume and economics justify it.

**Source:** Sacramento County EMD, Stormwater Program BMPs — Vehicle Washing  
https://emd.saccounty.gov/us/en/environmental-health/stormwater-compliance-program/stormwater-program-bmps.html

---

# 2. Lot archetypes

## Type A — Fenced paved/concrete IOS yard only

**Best initial use:** 5–20+ managed vehicles, depending on area and circulation.

What we need:

- gated perimeter;
- concrete or asphalt;
- water access;
- approved wastewater-disposal route;
- power or landlord approval for electrical upgrades;
- room for portable wash containment;
- small weatherproof equipment storage / container if no building;
- lighting / camera security.

### Advantages

- lowest rent and startup commitment;
- no need to pay for unused office/warehouse square footage;
- easy to scale from a small fenced section;
- excellent test of fleet flow, storage, inspection, and charging;
- lets CabOps / NexusOS pilot happen before a major facility commitment.

### Disadvantages

- bathrooms/office/storage may need a separate solution;
- exposed Sacramento heat/rain makes a canopy desirable;
- utility access may be limited;
- many IOS listings are intended only for storage, so washing/service use must be confirmed;
- a paved lot may drain directly to stormwater, requiring portable containment regardless of pavement quality.

### Current price benchmark — 3667 Omec Park Drive, Rancho Cordova

**Verified current listing:**

- M-2 heavy-industrial outdoor-storage site;
- fenced/gated yard space available from approximately 2,000 SF to 1 acre;
- asking **$0.16–$0.22/SF/month**.

Planning conversions:

| Yard size | Monthly rent at $0.16 | Monthly rent at $0.22 |
|---:|---:|---:|
| 2,000 SF | $320 | $440 |
| 5,000 SF | $800 | $1,100 |
| 10,000 SF | $1,600 | $2,200 |
| 12,115 SF | $1,938 | $2,665 |
| 13,500 SF | $2,160 | $2,970 |
| 15,000 SF | $2,400 | $3,300 |
| 0.50 acre / 21,780 SF | $3,485 | $4,792 |
| 1.00 acre / 43,560 SF | $6,970 | $9,583 |

The smaller sizes above are mathematical conversions of the published rate, not confirmation that each exact size is currently offered.

**Source:** LoopNet, 3667 Omec Park Dr  
https://www.loopnet.com/Listing/3667-Omec-Park-Dr-Rancho-Cordova-CA/36759870/

### Current Sacramento paved-yard example — Depot Park

**Verified current listing:**

- fenced and paved industrial yard lots;
- approximately **0.28–1.79 acres**;
- currently published individual areas include **12,115 SF, 13,500 SF, 18,912 SF, and 33,205 SF**;
- asking rate is **upon request**.

This is almost exactly the physical profile we should test for a first yard-only pilot. If its broker quote lands near the Omec market benchmark, the 12,115–13,500 SF lots could be especially interesting.

**Source:** LoopNet, Depot Park — Fenced & Paved Yard  
https://www.loopnet.com/Listing/Depot-Park-Sacramento-CA/13159517/

---

## Type B — Small warehouse / office plus small paved yard

**Best initial use:** development mule + detailing/inspection prototype + a small fleet.

This gives us:

- indoor secure equipment storage;
- restroom;
- workstation/NexusOS operator station;
- interior detailing bay or work area;
- weather protection;
- a small attached fleet yard.

The tradeoff is paying building rent for square footage that does not directly store vehicles.

### Current example — 8167 Belvedere Ave / Power Inn Industrial Park II

**Verified current listing:**

- 3,000 SF industrial suite;
- attached **3,000 SF fenced and paved yard**;
- **$3,450/month**;
- lease excludes utilities/property expenses/building services;
- M-2 industrial park context.

This yard is too small for a meaningful 20–50 vehicle depot, but it could work as a very early DEV-001 / software / wash-process development shop.

**Source:** LoopNet, Power Inn Industrial Park II  
https://www.loopnet.com/Listing/8160-14th-Ave-Sacramento-CA/30180151/

### Current example — 4141/4181 Power Inn Road

**Verified current listing:**

- 3,680 SF flex suite;
- approximately 1,970 SF attached fenced paved yard;
- **$4,526/month**;
- M-2S context;
- lease excludes utilities/property expenses/building services.

Useful as a shop/office, but again too little yard for the intended managed-fleet model.

**Source:** LoopNet, Power Inn Industrial Park  
https://www.loopnet.com/Listing/4141-Power-Inn-Rd-Sacramento-CA/15774826/

---

## Type C — Small real depot: warehouse plus 10,000–15,000 SF fenced paved yard

**Best initial use:** approximately 10–30 operational vehicles depending on final site layout.

This is currently the most attractive **balanced** archetype because it provides enough yard for a useful pilot while preserving indoor space for detailing, storage, operators, inspection equipment, and future maintenance.

### Current example — 3453 Ramona Ave, Sacramento

**Verified current listing:**

- 5,283 SF flex space;
- **10,680 SF attached fenced paved yard**;
- private office/open-office areas and restroom;
- **$7,185/month**;
- lease excludes utilities/property expenses/building services.

This is one of the best current examples of a first true CyberCab depot without immediately paying $15,000/month.

**Source:** LoopNet, Ramona Business Park  
https://www.loopnet.com/Listing/3453-Ramona-Ave-Sacramento-CA/13528920/

### Backup-market example — 506 Glide Ave, West Sacramento

**Verified current advertisement:**

- 3,100 SF industrial/retail building with two warehouse areas;
- **0.35-acre fully paved fenced yard**;
- M-1 zoning;
- bathroom/office/storage;
- advertised **$3,500/month**;
- listing specifically describes room for truck/van/fleet use.

This is an unusually cheap illustration of the building+yard archetype. It is a backup rather than the first choice because we want the Sacramento/SMUD-side operating economics when otherwise comparable sites exist.

**Source:** current August 26, 2026 Craigslist advertisement / broker post  
https://www.craigslist.org/view/d/sacramento-industrial-for-sale-lease/q9VzhDuWsVSN5t8DjqwyMQ

---

## Type D — Scalable depot: 0.75–1+ acre paved/concrete yard plus shop

**Best initial use:** roughly 30–80+ operational vehicles depending on site geometry and building footprint.

This is where fixed rent becomes meaningful enough that we should require committed vehicles / contracted MRR before signing.

### Current example — 3394 Sunrise Blvd, Rancho Cordova

**Verified current listing:**

- 6,000 SF freestanding warehouse;
- approximately **1.21 acres** total site;
- approximately **34,000 SF fenced paved concrete yard**;
- **$15,000/month + NNN**.

This is a strong picture of what a real early Hub 001 could look like after customer demand exists.

**Source:** JLL  
https://property.jll.com/listings/3394-sunrise-blvd-sacramento

### Current example — 7011 Power Inn Rd, Sacramento

**Verified current listing:**

- 5,000 SF industrial building;
- approximately **2-acre partially paved fenced yard** on a 2.69-acre parcel;
- three 18-foot grade-level doors;
- **$15,000/month NNN**;
- also offered for sale at **$3.2 million**.

This is substantially more capacity than we need for a first pilot, but it demonstrates that a much larger yard is available at the same published base rent as the smaller concrete-yard Rancho Cordova property.

**Source:** LoopNet  
https://www.loopnet.com/Listing/7011-Power-Inn-Rd-Sacramento-CA/37458437/

---

## Type E — Existing fleet/truck service property with wash rack

**Best use:** mature hub or an unusually favorable lease opportunity.

The valuable features are not aesthetics; they are avoided infrastructure cost:

- existing wash rack;
- oil/water separator;
- large electrical service;
- 480V three-phase;
- drive-through bays;
- paved secured yard.

### Current reference — 7080 Florin Perkins Rd, Sacramento

**Verified current listing:**

- approximately **8.54 acres**;
- approximately **372,002 SF fully paved secured yard**;
- approximately 19,650 SF total improvements;
- ten 12' x 14' grade-level doors with drive-through potential;
- **600A, 277/480V, three-phase** power listed, tenant to verify;
- **on-site wash rack with oil/water separator**;
- can accommodate approximately 200 trailers;
- asking rent: **contact broker**.

This is far too large to justify for our first operation, but it is an extremely useful mature-hub reference because it proves that the exact industrial features we want exist in the Sacramento market.

An existing compliant wash rack can be economically valuable because constructing a permanent industrial wash bay from scratch can easily become a six-figure project.

**Sources:** CBRE / LoopNet  
https://www.cbre.com/properties/properties-for-lease/industrial/details/US-SMPL-195297/7080-florin-perkins-road-sacramento-ca-95828  
https://www.loopnet.com/Listing/7080-Florin-Perkins-Rd-Sacramento-CA/39307021/

---

## Type F — Gravel / unpaved industrial yard

**Best use:** low-cost overflow parking/storage, not the preferred wash/inspection site.

Advantages:

- often cheaper;
- easier to find large acreage.

Disadvantages for CyberCab:

- dust contaminates vehicles immediately after cleaning;
- poor computer-vision environment;
- mud/track-out during rain;
- harder charger construction;
- harder striping/position control;
- less suitable for a professional owner-facing depot;
- vehicle washwater handling can be more complicated.

A gravel yard can still work if a smaller contained concrete wash/inspection/charging zone exists, but a fully paved yard is worth paying a reasonable premium for at our scale.

---

## Type G — Bare industrial land

**Best use:** later owner-developed hub, not first startup.

Bare land can look inexpensive per acre but usually forces us to buy the infrastructure ourselves: grading, drainage, pavement, fencing, lighting, water, sewer, electrical service, transformer upgrades, structures, wash bay, and entitlement work.

### Current example — 1 Rovana, Sacramento

**Verified current listing:**

- 12.03 acres;
- M-2S heavy industrial;
- electricity (SMUD), water and sewer described as stubbed to site;
- **$3.1 million asking price**;
- approximately **$257,689 per acre**.

That price is only the land. It does not create a usable depot.

**Source:** LoopNet  
https://www.loopnet.com/Listing/1-Rovana-Sacramento-CA/40602702/

### Decision

Do not buy/develop bare land for the first CyberCab depot unless an extraordinary financing/partnership structure changes the economics.

---

# 3. Sacramento industrial market context

Kidder Mathews' Q2 2026 Sacramento industrial report lists average direct NNN rents around:

- **Power Inn: $0.83/SF/month**;
- **McClellan: $0.71/SF/month**;
- **Natomas/Northgate: $0.81/SF/month**;
- **Davis/Woodland: $0.61/SF/month**;
- **Roseville/Rocklin: $1.06/SF/month**.

These are building-space market statistics and should **not** be confused with IOS yard rent, which can be priced very differently. The Omec current IOS listing at $0.16–$0.22/SF/month is therefore especially useful as a yard-specific planning benchmark.

**Source:** Kidder Mathews Sacramento Industrial Market Report Q2 2026  
https://kidder.com/market-reports/sacramento-industrial-market-report/

---

# 4. Planning capacity by yard size

These are **operational planning assumptions, not approved parking counts**.

A bare parking rectangle may use only ~160–180 SF per passenger car, but a robotaxi depot also needs circulation, charger clearance, wash/inspection space, turning, fire access, resource zones, and unusable geometry.

For early feasibility use:

- **350–450 gross yard SF per vehicle** as a planning band for an operational fleet yard;
- then reduce further if a building, wash bay, setbacks, fire lanes, or odd geometry consume yard area.

| Usable yard area | Gross planning capacity @ 450 SF/car | Gross planning capacity @ 350 SF/car | Practical early planning target |
|---:|---:|---:|---:|
| 3,000 SF | 6 | 8 | ~4–7 |
| 10,000 SF | 22 | 28 | ~15–25 |
| 12,000 SF | 26 | 34 | ~18–28 |
| 15,000 SF | 33 | 42 | ~22–35 |
| 0.50 acre / 21,780 SF | 48 | 62 | ~35–50 |
| 34,000 SF | 75 | 97 | ~50–75 |
| 1 acre / 43,560 SF | 96 | 124 | ~70–95 |
| 2 acres / 87,120 SF | 193 | 248 | ~140–190 before building/site constraints |

Actual capacity must later be established with a site plan, fire/access rules, charger geometry, accessibility, and local permitting.

---

# 5. Lean wash-equipment cost baseline

The startup system should prioritize throughput and compliance, not an expensive public-facing car-wash installation.

## Portable wash containment

Current commercial/mobile detailing containment-mat examples are available around **$550** for a 12' x 23' water-containment mat. Higher-spec complete wash-water recovery packages can cost several thousand dollars.

Planning allowance:

- containment mat: **$550–$1,500**;
- sump pump / wet-vac / hoses / drain barriers / recovery accessories: **$750–$2,500**;
- initial portable washwater compliance setup: **$1,300–$4,000**.

**Current product example:** All American Car Care Products water containment mat — $549.99  
https://allamericancarcareproducts.com/products/water-containment-mat-for-car-wash-and-mobile-detailing-12x23-car-wash-mat

## Commercial pressure washer

Current 4 GPM / 4,000 PSI commercial cold-water examples are roughly **$1,600–$3,100**, depending on drive type/pump/package. A belt-drive Pressure-Pro 4 GPM / 4,000 PSI unit is currently advertised around $1,899 at Home Depot; larger pro packages run higher.

Hot-water commercial systems are more like **$3,700–$5,600+** and are not necessary for the first normal exterior-wash workflow.

Planning decision:

- start with commercial cold-water equipment;
- add hot-water capability only if actual exception-cleaning data justifies it.

Sources/examples:  
https://www.homedepot.com/b/Outdoors-Outdoor-Power-Equipment-Pressure-Washers-Gas-Pressure-Washers/Commercial-Residential/4000-PSI/N-5yc1vZ2fkooylZ1z0y7p9Z1z1cap5  
https://www.homedepot.com/p/330992229

## Overhead boom

Commercial 360-degree stainless overhead booms currently show roughly **$950–$1,300** for common single-boom configurations; dual systems can be higher.

Examples:  
Mosmatic 9-foot 360-degree boom: approximately $1,126  
https://stores.kecsupplies.com/mosmatic-66-079-360-degree-ceiling-mount-boom-9-ft/  
Mosmatic 19-foot swing-diameter example: approximately $1,098  
https://www.watercannon.com/product/4525/car-wash-boom-overhead-360-degree-19-foot-swing-diameter-4000-psi

The boom itself is inexpensive. The support structure, plumbing, containment, electrical, and permitting are what can make a permanent wash bay expensive.

## Detailing vacuum

Professional wall-mountable wet/dry detailing systems are currently around **$1,000** for a complete premium wall-mounted setup; less expensive commercial shop-vac solutions are possible.

Example: Bigboi SuckR PRO+ wall-mounted set — $999  
https://ibigboi.com/products/suckr-pro-plus-portable-detailing-vacuum

## Initial equipment allowance

For a credible one-bay startup, excluding permanent construction:

| Item | Planning allowance |
|---|---:|
| Commercial cold-water pressure washer | $1,800–$3,100 |
| Portable wash containment/recovery | $1,300–$4,000 |
| Wet/dry detailing vacuum | $500–$1,000 |
| Extractor / steamer / interior tools | $1,000–$2,500 |
| Hoses, reels, foam, sprayers, carts | $1,000–$2,500 |
| DI/spot-free rinse setup | $500–$2,000 |
| Initial chemicals, towels, PPE, consumables | $1,000–$2,500 |
| Lighting / portable inspection equipment | $500–$2,000 |
| **Total** | **~$7,600–$19,600** |

These are planning allowances; exact equipment should be selected in a later vendor/equipment pass.

---

# 6. Permanent wash-bay cost warning

A permanent industrial wash bay is not simply a pressure washer under a roof.

A 2026 industrial construction benchmark places wash-bay projects around **$85,000–$200,000+** depending on slab, trench drainage, containment, oil/water separation, recycling, automation, and site work.

That is not a Sacramento contractor quote, but it is a useful order-of-magnitude warning.

**Source:** Wright Construction, 2026 Industrial Wash Bay Construction benchmark  
https://wrightconstructioninc.com/post/industrial-wash-bay-construction-concrete-compliance/

### Decision

Do **not** build a permanent wash bay at startup merely because we intend to wash vehicles.

Use portable compliant containment first. Buy/build permanent wash infrastructure when one of these becomes true:

1. throughput data shows portable/manual containment is a real bottleneck;
2. a lease term is long enough to justify the improvement;
3. the landlord funds/credits the improvement;
4. a permanent system materially lowers operating cost with acceptable payback;
5. customer commitments make the fixed cost supportable.

---

# 7. Charging startup economics

Tesla currently says a complete turnkey commercial Wall Connector installation typically runs **$2,000–$5,000 per unit**, including hardware, assessment, engineering, wiring, labor/materials, connectivity, permitting, and commissioning.

**Source:** Tesla Certified Installer quick guide  
https://energylibrary.tesla.com/docs/Public/Charging/WallConnector/What-to-Expect/en-us/GUID-C1A3CFA1-8C18-4BA4-ADD6-7254AA08D779.html

SMUD's current commercial EV incentives list:

- low-power Level 2 (<6.6 kW): **$2,500/handle**;
- high-power Level 2 (>=6.6 kW): **$3,500/handle**;
- non-public DCFC <50 kW: **$7,500/handle**;
- 50–149.9 kW DCFC: **$15,000/handle**;
- >=150 kW DCFC: **$30,000/handle**;
- stub-out: **$250**;
- panel upgrade: **$1,000**;
- transformer upgrade: **$5,000**;
- larger equity-site incentives may apply.

Incentives are subject to program requirements and cannot be treated as guaranteed cash until reserved/approved.

**Source:** SMUD Business EV program  
https://www.smud.org/driveelectricbusiness

### Critical lease rule

**Run a SMUD Grid Capacity Evaluation before signing a long-term lease that depends on fleet charging.**

SMUD says new EV load may require transformer/conductor upgrades and provides a preliminary Grid Capacity Evaluation, generally in **7–10 business days**. The result is informational and does not guarantee later construction capacity, but it is far better than leasing blind.

Source:  
https://www.smud.org/hm/-/media/Documents/Rebates-and-Savings-Tips/Go-Electric/Business/3778-GridCapacityEvaluation-Feb2025-fillable.ashx

### Startup charging decision

Because Cybercab-specific charging behavior remains an external capability gate:

- install only the number of Level 2 ports needed for DEV-001 / initial ordinary Tesla-managed fleet;
- design electrical/conduit capacity for expansion;
- use SMUD stub-outs where eligible;
- do not purchase a large DC fast-charging bank until the real Cybercab charging/turnaround requirement is known.

### Initial budget

- two Level 2 ports, gross turnkey: **~$4,000–$10,000**;
- four ports, gross turnkey: **~$8,000–$20,000**;
- potential SMUD incentive at high-power L2: up to **$3,500/handle**, subject to eligibility/project cost/reservation.

Do not net incentives to zero in the startup budget before approval.

---

# 8. California car-wash registration / bond

California's Department of Industrial Relations currently states that businesses engaged in car washing/polishing must register, with specified exceptions. The annual registration/assessment fee is **$300 per location**.

Current DIR required-document guidance states that a new applicant must maintain a **$150,000 surety bond**.

The $150,000 is the **bond face amount, not $150,000 of cash we necessarily deposit**. A current California surety vendor advertises the required $150,000 bond at a **$3,000 annual premium**; actual underwriting cost can vary.

Sources:  
DIR registration: https://dir.ca.gov/dlse/Car_Wash_Polishing.htm  
DIR required documents: https://www.dir.ca.gov/dlse/required_documents.html  
Example surety premium: https://www.nnasuretybonds.com/bonds/license-and-permit/wage-guarantee-and-employer-bonds/california-car-wash-bond

### Startup budget treatment

Conservatively reserve:

- state registration: **$300/location**;
- surety-bond premium: **~$3,000/year planning baseline**, with quote required;
- workers' compensation once employees are hired, as required;
- payroll/labor compliance administration.

If the operation is initially owner-only, confirm the precise registration treatment with DIR/counsel before relying on an exemption. The business plan should not depend on an aggressive exemption interpretation.

---

# 9. Insurance

Because we intend to store/manage customer-owned vehicles, ordinary general liability is not enough.

Garagekeepers coverage is designed for customer vehicles while in the business's care, custody, or control. Published small-auto-service averages can be low, but CyberCab's eventual aggregate customer-vehicle exposure can become much larger than a normal small detailer.

Current Insureon examples/medians include:

- car wash/detailing general liability: about $54/month median;
- car wash/detailing BOP: about $89/month median;
- car wash/detailing workers' comp: about $131/month median;
- auto-service garagekeepers: about $38/month median.

These are **not adequate budget quotes for a depot holding many customer vehicles overnight**. Insurers rate garagekeepers based in part on vehicle type, number of vehicles on site, location, and policy limits.

Sources:  
https://www.insureon.com/auto-services-business-insurance/car-detailing-wash/cost  
https://www.insureon.com/auto-services-business-insurance/garage-keepers-insurance

### Planning allowance

Until a broker quotes the actual custody exposure:

- reserve **$5,000–$15,000/year** for the first meaningful garage/depot insurance package;
- expect the number to rise with maximum simultaneous customer-vehicle value;
- obtain a quote specifically based on `maximum vehicles on site × replacement value`, not generic detailing revenue alone.

This is an allowance, not a market quote.

---

# 10. Local business-license baseline

### Rancho Cordova

Current general business license:

- first application: **$103**;
- renewal: **$103**.

Source:  
https://www.cityofranchocordova.org/businesses/business-resources/apply-for-a-license-or-permit-businesses/business-license

### Sacramento

Sacramento requires businesses operating in the city to obtain a Business Operations Tax certificate. Exact tax/fee depends on business classification and should be confirmed for the selected site.

Source:  
https://www.cityofsacramento.gov/finance/revenue/business-operations-tax

### West Sacramento

Current local-commercial business-license application is published around **$220.16** without the county-health fee and **$421.16** where county-health review applies.

Source:  
https://www.cityofwestsacramento.org/government/departments/community-development/business-licenses/business-license-applications/-fsiteid-1

Local business-license fees are small compared with rent, power, insurance, washwater, and vehicle-custody costs.

---

# 11. Startup scenarios

The following scenarios combine current verified prices with explicit planning allowances.

**These are cash-planning ranges, not contractor or landlord quotes.**

The largest unknowns remain:

- lease deposit / first-month / NNN structure;
- exact permitted use;
- water/sewer access;
- electrical upgrades;
- aggregate garagekeepers exposure;
- site-security requirements;
- payroll once staffed.

## Scenario A — Lean paved-yard pilot

**Target:** 5–15 managed vehicles.  
**Physical concept:** 10,000–15,000 SF fenced paved yard; portable wash containment; small storage container; 0–2 initial L2 chargers.

Current yard-rent benchmark from Omec:

- 10,000 SF: approximately **$1,600–$2,200/month**;
- 15,000 SF: approximately **$2,400–$3,300/month**.

### Startup cash model

| Component | Low | High | Type |
|---|---:|---:|---|
| Lease signing reserve (assume ~2–3 months base; landlord-specific) | $4,000 | $10,000 | allowance |
| Wash/detail equipment | $7,600 | $19,600 | allowance grounded in current equipment pricing |
| 0–2 L2 chargers | $0 | $10,000 | current Tesla turnkey range |
| DIR registration + bond premium reserve | $3,300 | $5,000 | current fee + allowance |
| Insurance initial/annual reserve | $5,000 | $15,000 | allowance; quote required |
| Security/access/storage | $1,500 | $5,000 | allowance |
| Legal/zoning/lease review/permitting preflight | $2,000 | $6,000 | allowance |
| Initial consumables already not captured above / contingency | $1,500 | $4,000 | allowance |
| Working-capital reserve | $12,000 | $30,000 | allowance |
| **Estimated startup cash** | **~$36,900** | **~$104,600** | planning range |

### Recommended capital target

Do not plan to launch this scenario with the mathematical minimum.

A safer working target is roughly:

> **$50,000–$80,000 available cash**

if the site requires no major electrical/sewer construction and initial staffing remains extremely lean.

If chargers are deferred and the founders provide most labor, it may be possible to start lower. If the lot lacks sanitary/water/power infrastructure, the site may be a false bargain and should be rejected rather than forcing expensive retrofits.

---

## Scenario B — Small building + useful paved yard

**Target:** roughly 10–25 managed vehicles.  
**Reference:** 3453 Ramona Ave at **$7,185/month**, with 5,283 SF building + 10,680 SF yard.

### Startup cash model

| Component | Low | High |
|---|---:|---:|
| Lease signing reserve (~2–3 months base assumption) | $14,400 | $21,600 |
| Wash/detail equipment | $9,000 | $20,000 |
| 2–4 L2 chargers, gross | $4,000 | $20,000 |
| Electrical/civil/site modifications | $3,000 | $20,000 |
| DIR/bond/local compliance | $3,500 | $6,000 |
| Insurance reserve | $6,000 | $15,000 |
| Security/cameras/access | $2,000 | $7,500 |
| Legal/zoning/lease/permitting | $2,500 | $7,500 |
| Working capital | $25,000 | $45,000 |
| **Estimated startup cash** | **~$69,400** | **~$162,600** |

### Recommended capital target

> **~$85,000–$125,000** if the site already has usable water/sewer/power and no permanent wash construction is required.

The upper end rises quickly if electrical upgrades or permanent civil work are needed.

---

## Scenario C — 0.75–1.25 acre scalable depot, portable/manual wash

**Target:** roughly 30–75 managed vehicles.  
**Reference:** 3394 Sunrise at **$15,000/month + NNN**, 34,000 SF concrete yard + 6,000 SF warehouse.

### Startup cash model — no permanent wash bay

| Component | Low | High |
|---|---:|---:|
| Lease signing reserve (~2–3 months base assumption) | $30,000 | $45,000 |
| Wash/detail/inspection equipment | $15,000 | $30,000 |
| 4–10 L2 chargers, gross | $8,000 | $50,000 |
| Electrical/civil/site work | $10,000 | $50,000 |
| Compliance/bond/legal/permitting | $6,000 | $15,000 |
| Insurance reserve | $10,000 | $25,000 |
| Security/access/camera infrastructure | $5,000 | $20,000 |
| Working capital | $50,000 | $100,000 |
| **Estimated startup cash** | **~$134,000** | **~$335,000** |

This is why a 1-acre depot should be pulled into existence by real customer commitments rather than leased speculatively.

---

## Scenario D — Scalable depot + newly constructed permanent wash bay

Take Scenario C and add approximately:

> **+$85,000 to $200,000+**

for permanent wash-bay infrastructure based on current industrial wash-bay construction benchmarks.

That creates a rough startup envelope of:

> **~$220,000–$535,000+**

before buying any Cybercabs.

### Decision

This is **not** the startup architecture unless we obtain a property where the wash infrastructure already exists or the landlord materially funds it.

---

# 12. Why an existing wash rack can justify higher rent

A property with:

- contained wash rack;
- approved sanitary-sewer connection;
- oil/water separator;
- canopy;
- three-phase service;
- large panel/transformer capacity;
- fenced paved yard;

can be worth materially more rent than a bare paved lot because it may avoid six figures of wash/electrical/civil work.

When comparing two properties, do **not** compare rent alone.

Use:

```text
EFFECTIVE 3-YEAR SITE COST
=
Rent
+ NNN / operating expenses
+ required tenant improvements
+ utility upgrades
+ washwater infrastructure
+ charger infrastructure
+ security
- landlord TI allowance
- utility incentives
- residual value / portability of equipment
```

A $10,000/month site with a compliant existing wash rack and strong 480V service can be cheaper over three years than a $5,000/month site requiring $150,000+ of non-portable civil work.

---

# 13. Lease vs buy

Current Sacramento-area asking examples show why leasing is preferred at startup:

| Property | Current asking sale price | Relevant features |
|---|---:|---|
| 7120 McCurdy Ln | $1.55M | 0.75 acres, 10,120 SF building, fenced paved yard |
| 11 Quinta Ct | $2.95M | 1.9 acres, 14,000 SF structures, fenced paved yard |
| 7011 Power Inn Rd | $3.20M | 2.69 acres, 5,000 SF building, ~2-acre fenced/partially paved yard |
| 1 Rovana | $3.10M | 12.03 acres bare/heavy-industrial land; utilities stubbed |

Sources:  
https://www.loopnet.com/Listing/7120-McCurdy-Ln-Sacramento-CA/41410947/  
https://www.loopnet.com/Listing/11-Quinta-Ct-Sacramento-CA/35003797/  
https://www.loopnet.com/Listing/7011-Power-Inn-Rd-Sacramento-CA/40581552/  
https://www.loopnet.com/Listing/1-Rovana-Sacramento-CA/40602702/

### Startup decision

**Lease first.**

Buying land/buildings consumes capital that can instead fund:

- working capital;
- charging infrastructure;
- software;
- fleet acquisition;
- customer acquisition;
- operating reserves.

Ownership can become attractive once depot utilization is proven and the owned-fleet balance sheet is strong enough to justify real estate as a long-duration infrastructure asset.

---

# 14. Property preflight checklist — before LOI or lease

No CyberCab site should be approved based only on listing photos/rent.

## Gate A — Land use

- zoning designation;
- outdoor vehicle storage allowed?
- vehicle washing/detailing allowed as primary/accessory use?
- maintenance allowed?
- future charger installation allowed?
- autonomous fleet staging allowed?
- 24/7 operation allowed?
- noise/light restrictions?
- conditional/minor use permit required?

## Gate B — Lease

- exact base rent;
- NNN/CAM estimate;
- annual escalation;
- deposit;
- personal/corporate guarantee;
- term and renewal options;
- landlord TI allowance;
- landlord approval for wash/EVSE/cameras/canopy;
- restoration obligations at lease end;
- signage/access rights;
- overnight customer-vehicle storage explicitly allowed.

## Gate C — Water / sewer

- water service and pressure;
- sanitary sewer available?
- exact legal washwater discharge point;
- sewer agency approval;
- storm drain locations;
- site drainage direction;
- oil/water separator already present?
- portable containment practical?

## Gate D — Electrical

- service voltage;
- phase;
- service amperage;
- panel spare capacity;
- transformer identity/capacity;
- distance from panel to intended chargers;
- trench/saw-cut requirements;
- landlord utility upgrade rights;
- SMUD Grid Capacity Evaluation before commitment.

## Gate E — Operations

- gate width;
- one-way circulation possible?
- vehicle turning geometry;
- inspection portal location;
- wash-mat location;
- clean/dirty flow separation;
- charger queuing;
- exception-bay location;
- employee restroom;
- secure chemical/equipment storage;
- towing/service access.

## Gate F — Security / insurance

- fence condition;
- gate automation;
- lighting;
- camera coverage;
- alarm/network availability;
- maximum simultaneous customer-vehicle value;
- garagekeepers underwriting approval before lease execution where possible.

---

# 15. Property scoring model

Use a 100-point score so cheap rent cannot hide a bad site.

| Category | Weight |
|---|---:|
| Utility / charging readiness | 25 |
| Washwater / sewer readiness | 20 |
| Zoning / permitted-use certainty | 15 |
| Rent + unavoidable fixed cost | 15 |
| Paved secured usable yard | 10 |
| Flow / ingress / egress | 5 |
| Existing building / restroom / storage | 5 |
| Expansion potential | 3 |
| Freeway/network position | 2 |
| **Total** | **100** |

### Automatic rejection conditions

Regardless of score, reject or heavily condition a property if:

- commercial fleet operation is not permitted;
- washwater cannot be legally handled at reasonable cost;
- utility capacity cannot support the pilot or a realistic expansion path;
- landlord prohibits EVSE/wash/security improvements;
- customer vehicles cannot be insured at the site;
- access geometry is unsafe or operationally unusable.

---

# 16. Current candidate / reference shortlist

## Tier 1 — investigate now for lean pilot

### Depot Park, Sacramento

Why:

- current 12,115 / 13,500 / 18,912 SF fenced paved lot options;
- ideal physical size for a 10–25 vehicle experiment;
- rate needs broker quote.

Questions:

- water?
- sanitary sewer access?
- washing/detailing permitted?
- electrical service available to individual lot?
- EVSE installation rights?

### 3667 Omec Park Dr, Rancho Cordova

Why:

- current published $0.16–$0.22/SF/month yard pricing;
- M-2 heavy-industrial outdoor-storage setting;
- can start small and expand.

Questions:

- is the yard paved/concrete in the exact offered section?
- water/sewer access?
- vehicle washing allowed?
- power availability?
- overnight customer-vehicle use?

### 3453 Ramona Ave, Sacramento

Why:

- building + 10,680 SF fenced paved yard;
- $7,185/month;
- useful first true depot scale.

Questions:

- electrical service details;
- sewer/washwater route;
- landlord approval for EVSE/wash process;
- exact NNN/property expenses.

## Tier 2 — lower-cost development or backup use

### 8167 Belvedere Ave

- $3,450/month;
- 3,000 SF building + 3,000 SF yard;
- good DEV-001/workshop footprint;
- too little fleet yard for long-term depot economics.

### 506 Glide Ave, West Sacramento

- $3,500/month advertised;
- 0.35-acre fully paved fenced yard + 3,100 SF building;
- very attractive rent, but treat as backup and compare utility economics/territory before selecting.

## Tier 3 — customer-demand-triggered hub

### 3394 Sunrise Blvd

- $15,000/month + NNN;
- 34,000 SF concrete yard;
- 6,000 SF warehouse;
- real 50+ vehicle potential.

### 7011 Power Inn Rd

- $15,000/month NNN;
- approximately 2 acres of partially paved/fenced yard;
- much more capacity than startup requires.

## Mature reference

### 7080 Florin Perkins Rd

- existing wash rack / oil-water separator;
- 600A 480V three-phase;
- 372,002 SF paved secured yard;
- ideal reference architecture, not a reasonable first lease unless pricing/sublease structure is unusually favorable.

---

# 17. Recommended physical startup architecture

Based on current Sacramento-area pricing and compliance rules, the preferred progression is now:

## Stage 0 — software / DEV-001

No depot lease yet.

Validate:

- Tesla integration;
- CabOps;
- CabEnergy;
- command/audit flow;
- simulator;
- owner workflow.

## Stage 1 — paved-yard pilot

Target:

- **~10,000–15,000 SF fenced paved/concrete yard**;
- approximately **$1,600–$3,300/month** yard-rent planning range using the current Omec IOS benchmark;
- portable compliant wash mat/recovery;
- portable/manual cold-water wash equipment;
- secure storage;
- cameras/lighting;
- 0–2 L2 chargers initially;
- no automatic car wash;
- no newly constructed permanent wash bay.

Capital target:

> **roughly $50,000–$80,000 available cash** if the property already gives us practical water, sewer, and electrical access.

## Stage 2 — small real depot

Trigger:

- contracted fleet volume outgrows yard-only workflow;
- customer commitments justify fixed rent;
- building becomes valuable for operations.

Target:

- building + **10,000–20,000 SF yard**;
- current reference rent roughly **$7,000/month** for a workable Sacramento example;
- 2–4+ managed chargers;
- more formal inspection/interior workflow.

Capital target:

> **roughly $85,000–$125,000** when major permanent civil work is unnecessary.

## Stage 3 — scalable Hub 001

Trigger:

- roughly 25–50+ committed vehicles / equivalent MRR depending final economics;
- enough contracted gross margin to cover fixed facility cost with safety margin;
- proven charging/wash throughput.

Target:

- 0.75–1.25+ acre usable yard;
- concrete/paved;
- warehouse/shop;
- meaningful electrical capacity;
- permanent wash infrastructure only if justified or already present.

Current reference rent:

> **~$15,000/month + NNN** for a 34,000 SF concrete-yard Rancho Cordova example.

---

# 18. Current decision

## Freeze candidate

**A fully paved/concrete fenced industrial yard is sufficient for the first CyberCab physical pilot when land use, wastewater, water, power, access, and insurance gates are satisfied.**

The first site does not need:

- an automatic car wash;
- a large warehouse;
- a permanent wash rack;
- DC fast charging;
- 1+ acre of land.

The highest-leverage startup site is likely **10,000–15,000 SF paved and fenced**, with a legal wastewater path and enough electrical capacity to support a small number of Level 2 chargers plus future stub-outs.

The site should be selected around **water/sewer/power/zoning**, not around appearance.

---

# 19. Open research gates

Before P0.20A can be promoted beyond a research baseline:

1. Call/obtain written quote for Depot Park 12,115 and 13,500 SF lots.
2. Call/obtain written quote and utility details for 3667 Omec offered sections.
3. Confirm accessory/commercial fleet washing land-use treatment with Sacramento and Rancho Cordova for shortlisted addresses.
4. Obtain sewer-discharge preflight for shortlisted addresses.
5. Submit/obtain SMUD Grid Capacity Evaluation for serious candidate meters/addresses.
6. Obtain a broker garagekeepers/garage liability quote for 10, 25, 50, and 100 customer vehicles on site.
7. Obtain commercial EVSE contractor ROM quote for 2, 4, 10 Level 2 ports plus future stub-outs.
8. Obtain washwater containment/recovery vendor quote.
9. Obtain exact landlord TI/deposit/NNN/electrical-improvement terms.
10. Build address-specific 10/25/50 vehicle layouts and confirm practical capacity.

---

# 20. Source register — accessed 2026-09-04

### Real estate

- LoopNet — 3667 Omec Park Dr: https://www.loopnet.com/Listing/3667-Omec-Park-Dr-Rancho-Cordova-CA/36759870/
- LoopNet — Depot Park: https://www.loopnet.com/Listing/Depot-Park-Sacramento-CA/13159517/
- LoopNet — Power Inn Industrial Park II: https://www.loopnet.com/Listing/8160-14th-Ave-Sacramento-CA/30180151/
- LoopNet — 4141 Power Inn: https://www.loopnet.com/Listing/4141-Power-Inn-Rd-Sacramento-CA/15774826/
- LoopNet — 3453 Ramona Ave: https://www.loopnet.com/Listing/3453-Ramona-Ave-Sacramento-CA/13528920/
- JLL — 3394 Sunrise Blvd: https://property.jll.com/listings/3394-sunrise-blvd-sacramento
- LoopNet — 7011 Power Inn: https://www.loopnet.com/Listing/7011-Power-Inn-Rd-Sacramento-CA/37458437/
- CBRE — 7080 Florin Perkins: https://www.cbre.com/properties/properties-for-lease/industrial/details/US-SMPL-195297/7080-florin-perkins-road-sacramento-ca-95828
- LoopNet — 1 Rovana: https://www.loopnet.com/Listing/1-Rovana-Sacramento-CA/40602702/
- LoopNet — 7120 McCurdy Ln: https://www.loopnet.com/Listing/7120-McCurdy-Ln-Sacramento-CA/41410947/
- LoopNet — 11 Quinta Ct: https://www.loopnet.com/Listing/11-Quinta-Ct-Sacramento-CA/35003797/
- Kidder Mathews Q2 2026 Sacramento industrial report: https://kidder.com/market-reports/sacramento-industrial-market-report/

### Washwater / regulation

- Sacramento County EMD stormwater BMPs: https://emd.saccounty.gov/us/en/environmental-health/stormwater-compliance-program/stormwater-program-bmps.html
- California DIR car wash registration: https://dir.ca.gov/dlse/Car_Wash_Polishing.htm
- California DIR required documents: https://www.dir.ca.gov/dlse/required_documents.html

### Charging

- SMUD Business EV incentives: https://www.smud.org/driveelectricbusiness
- SMUD Grid Capacity Evaluation: https://www.smud.org/hm/-/media/Documents/Rebates-and-Savings-Tips/Go-Electric/Business/3778-GridCapacityEvaluation-Feb2025-fillable.ashx
- Tesla commercial charging install guidance: https://energylibrary.tesla.com/docs/Public/Charging/WallConnector/What-to-Expect/en-us/GUID-C1A3CFA1-8C18-4BA4-ADD6-7254AA08D779.html

### Equipment / insurance

- All American containment mat: https://allamericancarcareproducts.com/products/water-containment-mat-for-car-wash-and-mobile-detailing-12x23-car-wash-mat
- Home Depot commercial pressure-washer category: https://www.homedepot.com/b/Outdoors-Outdoor-Power-Equipment-Pressure-Washers-Gas-Pressure-Washers/Commercial-Residential/4000-PSI/N-5yc1vZ2fkooylZ1z0y7p9Z1z1cap5
- Mosmatic boom example: https://stores.kecsupplies.com/mosmatic-66-079-360-degree-ceiling-mount-boom-9-ft/
- Insureon detailing/car-wash insurance costs: https://www.insureon.com/auto-services-business-insurance/car-detailing-wash/cost
- Insureon garagekeepers: https://www.insureon.com/auto-services-business-insurance/garage-keepers-insurance

---

# Change Log

## 2026-09-04 — v0.1

- Added current Sacramento/Rancho Cordova/West Sacramento property examples and published rents.
- Established paved/concrete yard-only pilot as a valid physical startup archetype subject to land-use, washwater, power, and insurance gates.
- Added portable washwater containment strategy based on Sacramento County official BMPs.
- Added current Tesla commercial Level 2 installation benchmark and SMUD commercial EV incentives.
- Added current California car-wash registration/bond requirements.
- Added staged startup-cost models for yard-only, building+yard, scalable depot, and permanent wash-bay cases.
- Added property scoring/preflight model.
- Added explicit lease-first decision and rejected bare-land development as the default startup path.
- Added current candidate/reference shortlist and next verification gates.
