# CyberCab Detail + Cab
## Investor Portfolio & Sacramento Pilot Proposal

**Date:** September 4, 2026  
**Status:** Investor discussion draft / working business name  
**Recommended initial capital raise:** **$150,000**  
**Primary launch geography:** Sacramento / Rancho Cordova, California  

> CyberCab Detail + Cab is an independent concept and is not affiliated with or endorsed by Tesla. "Cybercab" is a Tesla product name; public-facing brand/trademark review is required before commercial launch.

## Executive summary
CyberCab Detail + Cab is designed to become the outsourced operations layer for privately owned and third-party autonomous vehicle fleets. The core customer is an asset owner who wants robotaxi income without personally cleaning, charging, inspecting, storing, maintaining, recovering, or scheduling the vehicle.

The launch strategy is deliberately capital-efficient: lease a 10,000-15,000 SF fenced paved industrial yard in Sacramento or Rancho Cordova, begin with a portable/manual contained wash system and 0-2 Level 2 chargers, validate Tesla Fleet API / Fleet Telemetry integration with an existing authorized Tesla, and onboard managed vehicles before moving into a larger depot.

The company is not betting the seed round on owning vehicles. The proposed $150,000 seed pilot **excludes Cybercab purchases**. Vehicle acquisition becomes a later capital-allocation decision only after commercial pricing, fleet participation rules, unit economics, and customer demand are validated.

## Investment thesis
- Autonomous ride-hailing is moving from demonstration to scaled commercial operations. Waymo reported more than 400,000 rides per week in early 2026 and public fully autonomous service in 14 cities by September 2026.[23][24]
- Tesla is explicitly soliciting interest in **Cybercab fleet vehicle purchasing** and **mobility hubs and infrastructure** and now directs parties interested in commercial Cybercab fleets to its contact form.[1][2]
- Physical fleet operations remain an unsolved scaling bottleneck. Aseon Labs raised $10 million in 2026 to build distributed robotaxi cleaning/charging/inspection pit stops, and Zoox is opening new depots as it expands.[25][26]
- Sacramento offers unusually attractive launch economics: SMUD reports materially lower commercial electricity rates than neighboring PG&E categories and offers commercial EV charging incentives; industrial yard rents can be far below warehouse rents.[7][8][10][11]
- Tesla already exposes useful fleet software primitives, including customer-authorized Fleet API access, business Vehicle Manager relationships, telemetry, charging commands, and low-cost streaming.[4][5][6]

## Capital request
### Recommended seed pilot: $150,000

| Use | Budget |
|---|---:|
| Facility deposit + 12-month base occupancy reserve | $40,000 |
| Manual wash/detail/recovery equipment | $18,000 |
| Charging, electrical preflight, initial EVSE | $15,000 |
| Insurance, licensing, surety-bond premium, legal/compliance | $15,000 |
| Security, networking, site IT and equipment storage | $8,000 |
| CabOps / Tesla telemetry integration / hosting / test hardware | $12,000 |
| Labor and customer-onboarding working capital | $30,000 |
| Contingency | $12,000 |
| **Total** | **$150,000** |

The budget intentionally does not subtract rebates before approval. If eligible, SMUD currently advertises $3,500 per high-power Level 2 charging handle and larger incentives for non-public DC fast charging.[7]

### Alternative capitalization levels
- **$100,000 lean floor:** feasible only with a very low-cost yard, limited charger work, founder-deferred compensation, and tight contingency. Higher execution risk.
- **$150,000 recommended:** enough to build the correct pilot without immediately forcing a warehouse lease or permanent automated car wash.
- **$225,000 accelerated:** enables a larger site or small building, 4-6 Level 2 chargers, more staffing, and faster physical scaling, but should be used only after meaningful customer commitments.

## Problem
Autonomous vehicles can drive themselves, but they cannot yet eliminate the physical service layer. Fleet owners still need cleaning, charging, storage, inspection, incident handling, maintenance coordination, lost-property control, damage documentation, and readiness management.

The customer's desired product is not a wash. It is **passive autonomous fleet ownership**:

> You own the fleet. We operate the fleet.

## Solution stack
1. **CabCare** - routine exterior wash, interior reset, deep-clean exceptions, emergency spills, lost-property handling, damage documentation.
2. **CabDepot** - secure storage, charging, inspection, bay/resource management and return-to-service readiness.
3. **CabOps** - end-to-end outsourced fleet operations, owner reporting, maintenance coordination, charging/readiness planning, incident workflows and network routing.
4. **Owned Fleet** - once unit economics are proven, excess cash can compound into company-owned autonomous vehicles using infrastructure already funded by the service business.

## Sacramento launch thesis
Sacramento is selected because the business needs inexpensive industrial land, electrical capacity, highway connectivity and favorable charging economics more than expensive consumer-facing real estate.

Current September 2026 examples show the cost ladder clearly:
- 3667 Omec Park Drive: M-2 outdoor storage marketed at **$0.16-$0.22/SF/month**.[11]
- 3453 Ramona Avenue: 5,283 SF flex space plus 10,680 SF paved fenced yard at **$7,185/month**.[12]
- 3394 Sunrise Boulevard: 6,000 SF warehouse plus about 34,000 SF paved concrete yard at **$15,000/month + NNN**.[13]
- 7011 Power Inn Road: 5,000 SF building plus about 2 acres partially paved/fenced at **$15,000/month NNN**.[14]
- 6750 Florin Perkins Road: 27,360 SF industrial space, 22,650 SF fenced paved yard and 800A/480V/3-phase power at **$22,709/month**.[15]
- 7080 Florin Perkins Road: future-hub reference with about 372,000 SF paved secured yard, 600A/480V power and an existing wash rack.[16]

CBRE reported a Sacramento industrial average asking rent of $0.81/SF/month NNN in Q2 2026.[10] A yard-first launch can therefore preserve capital while still giving the company secure vehicle flow, charging and cleaning capacity.

## Why a paved yard is enough for the pilot
The first site does not need a permanent tunnel car wash. Sacramento County prohibits business vehicle washwater from entering the stormwater system, but explicitly describes portable wash pads, containment booms, vacuum recovery and approved sanitary-sewer disposal as compliance alternatives.[17]

This supports a first-stage facility with:
- secure paved/fenced yard;
- canopy/manual overhead wash station;
- portable containment and wastewater recovery;
- wet/dry vacuum, extractor/steamer, detailing carts and spot-free rinse;
- cameras and lighting;
- 0-2 Level 2 charging positions initially;
- reserved physical/electrical envelope for future automation.

Permanent automated washing is purchased only when measured throughput and payback justify it.

## Technology and defensibility
CyberCab Detail + Cab is being designed as an infrastructure-and-software company, not a detailing shop.

### CabOps architecture
- provider-neutral core;
- Tesla isolated behind a `TeslaAdapter`;
- Fleet Telemetry preferred over frequent polling;
- durable command ledger with request, acknowledgement and physical-effect verification;
- multi-tenant owner/manager/payer identity model;
- readiness state with explicit reason codes;
- simulator and real vehicles use the same contracts.

### Core modules
- **CabEnergy:** vehicle charging and site-energy optimization;
- **CabVision:** inspection, trash/spill/damage/lost-item detection;
- **CabCare:** cleaning workflows;
- **CabMaint:** maintenance restrictions and work orders;
- **CabIncident:** spill/damage/lost-property exceptions;
- **CabRoute:** depot/node routing and deadhead reduction;
- **CabDepot:** physical hub digital twin, bays, chargers and capacity;
- **CabRevenue:** owner statements, costs and unit economics;
- **CabDispatch:** readiness and operating state.

Tesla's current Fleet API already supports customer-authorized access and business Vehicle Manager relationships. Tesla also prices streaming telemetry far below repeated polling, making a production telemetry-first architecture economically practical.[4][5][6]

## Proposed commercial pricing - validation targets
Pricing is not frozen until customer interviews and contracts validate willingness to pay.

| Product | Proposed starting range | Notes |
|---|---:|---|
| CabCare | $249-$349 / vehicle / month | routine cleaning/reset; deep incidents extra |
| CabDepot | $549-$749 / vehicle / month | storage, normal care, inspection, charging management; electricity separate |
| CabOps | $799-$1,099 / vehicle / month | full management; alternative hybrid revenue-share contract may be offered |
| Activation | $500-$1,500 / vehicle | baseline inspection, configuration, tags/hardware, reserve setup |
| Emergency clean | $75-$250+ / event | severity-based |
| Electricity | metered pass-through or transparent markup | excluded from service ARPU model |

For modeling, the proposal uses a weighted **$775/month Year-1 managed-service ARPU**, excluding electricity, extraordinary incidents, maintenance pass-through and any future percentage-of-ride-revenue fee.

## Pilot unit economics - illustrative
Base pilot assumptions:
- monthly service ARPU: $775;
- direct vehicle servicing cost: $225/month;
- fixed pilot operating cost: about $11,000/month, including facility, loaded core labor and overhead;
- unit contribution before facility/core team: about $550/month.

**Illustrative break-even: approximately 20 managed vehicles.**

Sensitivity:
| Monthly ARPU | Approx. break-even vehicles* |
|---:|---:|
| $650 | 26 |
| $700 | 24 |
| $775 | 20 |
| $850 | 18 |
| $950 | 16 |

\*Using $225 direct cost per managed vehicle and $11,000 monthly fixed operating cost. These are planning assumptions, not guaranteed results.

## Illustrative 3-year operating model
The model intentionally excludes vehicle-owner ride revenue, pass-through electricity, extraordinary incident fees, future software licensing and company-owned robotaxi revenue.

| | Year 1 | Year 2 | Year 3 |
|---|---:|---:|---:|
| Avg. managed vehicles | 15 | 45 | 80 |
| Year-end managed vehicles | 30 | 60 | 100 |
| Weighted monthly service ARPU | $775 | $800 | $850 |
| Total revenue | $162,000 | $454,500 | $846,000 |
| Direct vehicle costs | $40,500 | $113,400 | $192,000 |
| Facility + core labor + overhead | $132,000 | $258,000 | $468,000 |
| **Illustrative EBITDA** | **-$10,500** | **$83,100** | **$186,000** |

The Year-3 model assumes moving into a larger hub-class property. A 100-vehicle portfolio at $850/month represents about **$1.02 million of annual recurring service revenue** before one-time fees or upside revenue streams.

## Go-to-market
### Customer beachhead
The first target is the small private fleet owner: an individual or small business willing to buy 1-10 robotaxis if ownership can be operationally passive.

### Acquisition sequence
1. Conduct 15-25 structured owner interviews and collect LOIs/reservations.
2. Submit Tesla Cybercab fleet/mobility-hub interest and Tesla developer app requests.[1][6]
3. Use an authorized existing Tesla as `DEV-001` to validate telemetry, commands, charging and CabOps workflows before Cybercab delivery.
4. Secure a 10,000-15,000 SF paved Sacramento/Rancho Cordova yard only after customer and utility preflight gates are met.
5. Onboard first 5-10 vehicles; prove cleaning times, charging readiness, incident handling and reporting.
6. Grow to 20-30 managed vehicles and operating break-even.
7. Expand into a small depot or hub only when contracted demand pulls the next facility into existence.

## 12-month milestone plan
**0-90 days**
- finalize legal/business structure and public brand review;
- Tesla developer and commercial fleet outreach;
- DEV-001 telemetry/command proof;
- 15-25 customer discovery interviews;
- first LOIs/reservations;
- 3-5 Sacramento property candidates with zoning, landlord and SMUD preflight.

**90-180 days**
- lease pilot yard if gates are met;
- install security, contained manual wash system and initial charging;
- launch CabOps operator console V0;
- onboard first 5-10 managed vehicles when provider/network rules permit.

**6-12 months**
- 20-30 managed vehicles target;
- readiness SLA target >=98%;
- routine reset labor target <=15 minutes per vehicle;
- CabEnergy deterministic scheduling V1;
- owner statements and incident audit trail;
- decision gate on larger depot / automated wash / first owned fleet asset.

## Market validation, not speculative TAM
This proposal avoids a giant top-down robotaxi TAM number. It uses observable operating activity and a unit-based revenue wedge instead.

- Waymo reported 400,000+ weekly rides and 15 million rides during 2025; by September 2026 it was serving fully autonomous trips in 14 cities.[23][24]
- Tesla's current Robotaxi product lists multiple U.S. cities and has begun limited Cybercab rides in Austin; Tesla is actively collecting fleet purchase and mobility-hub interest.[1][2][3]
- Aseon Labs' $10 million seed round specifically targets robotaxi charging/cleaning/inspection infrastructure.[25]
- Zoox is building additional depots as it expands.[26]

This validates the category while leaving open which AV provider ultimately supplies the largest privately managed fleet opportunity.

## Competitive position
### We do not compete with autonomy stacks
The company does not attempt to build FSD, Waymo Driver or a vehicle platform. It monetizes the physical and operational layer around autonomous assets.

### Initial moat
- customer contracts and owner history;
- Sacramento site/network and utility relationships;
- provider-neutral CabOps software;
- cleaning and inspection SOPs plus cycle-time data;
- energy optimization and site demand data;
- cross-vehicle operating dataset;
- regional nodes that reduce deadhead;
- company-owned fleet that later benefits from already-funded infrastructure.

The closest emerging infrastructure category is Aseon Labs' distributed robotaxi pit-stop model.[25] Our wedge is **passive fleet ownership and full managed operations for asset owners**, with software and physical infrastructure designed together rather than robotics hardware alone.

## Regulatory and execution risks
| Risk | Impact | Mitigation |
|---|---|---|
| Private Cybercab fleet/network rules change | High | provider-neutral CabOps; no seed capital spent on vehicle purchases; explicit capability gates |
| California AV deployment rules | High | operate only within provider/legal permissions; maintain DMV/CPUC/provider gate register |
| CyberCab naming/trademark | Medium/High | working name only; complete legal brand clearance before commercial launch |
| Site power insufficient | High | SMUD grid-capacity/preflight before lease; initial Level 2 scale; charger power sharing |
| Washwater/noncompliance | High | portable containment, recovery and approved disposal; site-specific EMD/sewer validation |
| Customer concentration | Medium | multiple small fleet owners; contract minimums; owner portal and service SLAs |
| Vehicle care/custody insurance | High | garagekeepers/garage liability and aggregate vehicle-value underwriting before onboarding |
| Labor scaling | Medium | exception-based cleaning, standardized 10-15 minute reset target, automation only at bottlenecks |
| Software/provider outages | Medium | durable state, command audit, telemetry reconciliation, human fallback |

California's current car wash rules also require registration for covered businesses, including a $300 annual per-location fee, and current new-applicant materials call for a $150,000 surety bond subject to exceptions. The bond amount is not the same as cash collateral; the actual premium/underwriting requirement must be quoted before launch.[18][19]

## Why the raise is capital-efficient
The recommended $150,000 seed pilot buys the operating system and the learning environment, not speculative fleet inventory.

It is designed to prove:
- customers will pay for passive fleet ownership;
- a paved-yard micro-depot is sufficient;
- routine cleaning can be standardized to a low labor-minute target;
- CabEnergy can keep the right vehicles ready without excessive peak demand;
- Tesla/provider APIs can be safely integrated;
- the business reaches contribution break-even at about 20 managed vehicles;
- larger property and automation are pulled by contracted demand rather than optimism.

## Capital expansion plan
**Seed Pilot - $150k:** prove the operating model and reach 20-30 managed vehicles.  
**Growth Facility - estimated $250k-$400k later:** move to 0.75-1.25+ acre hub, larger charging bank, additional staffing and automated inspection when contracts justify it.  
**Vehicle Acquisition - separate:** Cybercab/AV purchases are intentionally not included until commercial pricing, financing, network rules and actual fleet returns are known. An SPV or separate asset-financing structure may eventually be more appropriate than using operating-company equity for every vehicle.

## Long-term goal
Build a Sacramento-first autonomous fleet services network where a vehicle can choose the nearest qualified hub/node for charging, cleaning, inspection or maintenance rather than deadheading to one distant depot.

The mature company can combine:
- recurring managed-fleet revenue;
- software/API revenue;
- energy optimization;
- distributed service nodes;
- partner/franchise infrastructure;
- and an owned autonomous fleet.

The strategic end state is not "a car wash for Cybercabs." It is **the operating infrastructure that makes autonomous fleet ownership passive and scalable**.

## Investor note
This document is a planning proposal, not a securities offering, valuation opinion, promise of returns, or legal/tax advice. Investment instrument, valuation, governance rights and securities compliance are intentionally left open for counsel and investor negotiation. All future Cybercab revenue, availability and API functionality remain subject to Tesla/provider rules and applicable law.

## References

1. **Tesla Robotaxi interest form.** Tesla is soliciting interest in Cybercab fleet vehicle purchasing and mobility hubs/infrastructure. https://www.tesla.com/robotaxi/interest

2. **Tesla Cybercab FAQ.** Tesla says parties interested in an individual or fleet Cybercab for commercial purposes can submit the interest form. https://www.tesla.com/support/robotaxi/cybercab

3. **Tesla Robotaxi.** Tesla lists current Robotaxi cities and describes its autonomous fleet. https://www.tesla.com/robotaxi

4. **Tesla Fleet API - Third-Party Business Tokens.** Business authorization can include vehicles available through a Vehicle Manager relationship in Tesla for Business. https://developer.tesla.com/docs/fleet-api/authentication/third-party-business-tokens

5. **Tesla Fleet API pricing.** Fleet Telemetry is 150,000 signals per $1; commands are 1,000 requests per $1; polling and wakes cost more. https://developer.tesla.com/

6. **Tesla Fleet Telemetry.** Tesla recommends streaming telemetry rather than repeated vehicle_data polling for efficient fleet state ingestion. https://developer.tesla.com/docs/fleet-api/fleet-telemetry

7. **SMUD Business EV incentives.** Current commercial incentives include $3,500 per high-power Level 2 handle and larger DC fast-charging incentives for eligible projects. https://www.smud.org/driveelectricbusiness

8. **SMUD business rate comparison.** As of March 1, 2026, SMUD reports business rates roughly 48-53% below PG&E for many small/medium commercial categories. https://www.smud.org/Rate-Information/Business-rates

9. **Tesla commercial Wall Connector installation guide.** Tesla states a complete turnkey commercial Wall Connector installation typically ranges from $2,000-$5,000 per unit. https://energylibrary.tesla.com/docs/Public/Charging/WallConnector/What-to-Expect/en-us/GUID-C1A3CFA1-8C18-4BA4-ADD6-7254AA08D779.html

10. **CBRE Sacramento Industrial Q2 2026.** Sacramento industrial average asking rent was $0.81/SF/month NNN with 6.0% vacancy in Q2 2026. https://www.cbre.com/insights/figures/sacramento-industrial-figures-q2-2026

11. **3667 Omec Park Drive.** Current Rancho Cordova M-2 outdoor-storage yard space is marketed at $0.16-$0.22/SF/month. https://www.loopnet.com/Listing/3667-Omec-Park-Dr-Rancho-Cordova-CA/36759870/

12. **3453 Ramona Avenue.** 5,283 SF flex space plus 10,680 SF fenced paved yard is marketed at $7,185/month, excluding utilities/property expenses/building services. https://www.loopnet.com/Listing/3453-Ramona-Ave-Sacramento-CA/13528920/

13. **3394 Sunrise Boulevard.** 6,000 SF warehouse on 1.21 acres with about 34,000 SF fenced paved concrete yard is marketed at $15,000/month plus NNN. https://property.jll.com/listings/3394-sunrise-blvd-sacramento

14. **7011 Power Inn Road.** 5,000 SF building on 2.69 acres with about 2 acres of partially paved/fenced yard is marketed at $15,000/month NNN. https://www.loopnet.com/Listing/7011-Power-Inn-Rd-Sacramento-CA/37458437/

15. **6750 Florin Perkins Road.** 27,360 SF available industrial space with 22,650 SF fenced paved yard, 800A 480V 3-phase power, marketed at $22,709/month. https://www.loopnet.com/Listing/6750-Florin-Perkins-Rd-Sacramento-CA/39001663/

16. **7080 Florin Perkins Road.** Large Sacramento fleet/logistics site has about 372,002 SF secured paved yard, 600A 277/480V 3-phase power, and an on-site wash rack. https://www.cbre.com/properties/properties-for-lease/industrial/details/US-SMPL-195297/7080-florin-perkins-road-sacramento-ca-95828

17. **Sacramento County stormwater BMPs.** Businesses may not discharge vehicle washwater to storm drains; portable wash pads, containment, vacuum recovery, and sanitary-sewer disposal are among compliance alternatives. https://emd.saccounty.gov/us/en/environmental-health/stormwater-compliance-program/stormwater-program-bmps.html

18. **California car wash registration.** California requires covered car washing/polishing businesses to register; current fee is $300 per location. https://www.dir.ca.gov/dlse/Car_Wash_Polishing.htm

19. **California car wash required documents.** Current new-applicant documentation calls for a $150,000 surety bond, subject to stated exceptions. https://dir.ca.gov/dlse/required_documents.html

20. **California minimum wage.** California minimum wage is $16.90/hour in 2026 and is scheduled to increase to $17.40/hour January 1, 2027. https://dir.ca.gov/dlse/minimum_wage.htm

21. **California DMV autonomous vehicle rules.** California adopted updated AV regulations in April 2026; deployment remains permit- and provider-dependent. https://www.dmv.ca.gov/portal/news-and-media/new-autonomous-vehicle-regulations-strengthen-oversight-and-enforcement-authorize-trucks-and-transit/

22. **California DMV AV permit resources.** California maintains separate testing and deployment permit resources for autonomous vehicle manufacturers. https://www.dmv.ca.gov/portal/vehicle-industry-services/autonomous-vehicles/autonomous-vehicles-program-permit-resources/

23. **Waymo 2026 funding/growth update.** Waymo reported more than 400,000 rides per week across six U.S. metro areas and plans for 20+ additional cities in 2026. https://waymo.com/blog/2026/02/waymo-raises-usd16-billion-investment-round/

24. **Waymo September 2026 expansion.** Waymo began public rider access in Denver, San Diego, and Tampa, bringing fully autonomous trips to 14 cities. https://waymo.com/blog/2026/09/ride-in-denver-san-diego-tampa/

25. **Aseon Labs seed round.** Aseon Labs raised $10 million to develop distributed robotaxi pit stops for inspection, cleaning, and charging, validating the service-infrastructure category. https://techcrunch.com/2026/06/26/this-silicon-valley-startup-has-raised-10m-to-build-pitstops-to-clean-and-charge-robotaxis/

26. **Zoox expansion / depots.** Zoox announced new depots in Phoenix and Dallas as it expanded its robotaxi testing footprint, another signal that depot infrastructure is integral to AV scaling. https://zoox.com/journal/zoox-expanding-testing-dallas-phoenix
