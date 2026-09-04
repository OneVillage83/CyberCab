# CyberCab Project Log

This file is the durable chronological record for material CyberCab decisions and changes.

---

## 2026-09-04 — Repository initialized

**Area:** project / documentation

**Changed:**
- Initialized `OneVillage83/CyberCab` as the working repository for CyberCab Detail + Cab.
- Added repository overview and project map.
- Established documentation-first development rules.

**Reason:**
- Preserve project decisions as they are made instead of reconstructing architecture and business logic from prior conversations later.

**Open gates / follow-up:**
- Continue formalizing the Phase 0 architecture.
- Add P0.10B CabOps Core Domain & Event Contracts next.

**Affected docs/code:**
- `README.md`
- `docs/DOCUMENTATION_POLICY.md`
- `docs/PROJECT_LOG.md`

---

## 2026-09-04 — Business thesis formalized

**Area:** business / operations

**Changed:**
- Defined the business as more than a detailing service.
- The core customer promise is **passive robotaxi ownership**: the owner provides the vehicle/capital while CyberCab Detail + Cab handles the physical and operational burden.
- Established four working service layers: CabCare, CabDepot, CabOps, and Owned Fleet.
- Preserved the long-term capital-allocation flywheel in which excess operating profit can fund company-owned Cybercabs/autonomous vehicles.

**Reason:**
- Customer discovery began from a real owner profile: someone interested in earning robotaxi income but unwilling to personally clean, charge, maintain, or manage the vehicle.

**Open gates / follow-up:**
- Validate pricing and owner willingness-to-pay with additional customer interviews and LOIs.
- Build the owner-economics simulator before final pricing is frozen.

**Affected docs/code:**
- `README.md`
- `docs/phase-0/PHASE_0_OVERVIEW.md`

---

## 2026-09-04 — Sacramento-first hub strategy adopted

**Area:** facility / expansion / energy

**Changed:**
- Sacramento, California is the primary market and intended Hub 001 geography.
- Surrounding Sacramento-area industrial zones are acceptable backups.
- The long-term network may use smaller service nodes along Northern California corridors rather than requiring every location to be a full depot.
- Initial expansion concepts include west toward Davis/Vacaville, south toward Stockton/Modesto, and northeast toward Roseville/Rocklin/Auburn, subject to actual autonomous-operating permissions and economics.
- Property selection must prioritize electrical capacity, appropriate sewer/washwater handling, circulation, security, and expandability rather than appearance.

**Reason:**
- Sacramento can function as a lower-cost operational center while preserving access to major Northern California corridors.
- Charging-heavy fleet economics make site power and utility territory strategically important.

**Open gates / follow-up:**
- Build address-specific property/power database.
- Validate candidate electrical-service capacity directly with SMUD or the applicable utility before lease commitments.
- Validate zoning, washwater, permitting, insurance, and fleet-operation requirements.

**Affected docs/code:**
- `docs/phase-0/PHASE_0_OVERVIEW.md`

---

## 2026-09-04 — Manual-first wash architecture adopted

**Area:** facility / operations / capex

**Changed:**
- Initial facility should use a covered/manual overhead wash station rather than immediately buying an automated rollover/touchless machine.
- The site should still reserve the physical envelope, drainage, power, water, and circulation needed for a future automated system.
- Automatic wash equipment becomes an economically triggered upgrade rather than a startup requirement.

**Reason:**
- Chinese commercial automatic wash systems can be materially cheaper than traditional U.S. tunnel installations, but machine cost is only part of installed cost.
- Early capital is better spent only when fleet throughput proves the manual wash is a bottleneck.

**Open gates / follow-up:**
- Define exact utilization/payback trigger for automation.
- Build vendor and installed-cost matrix.
- Validate wastewater/reclamation/permitting requirements for each candidate site.

**Affected docs/code:**
- `docs/phase-0/PHASE_0_OVERVIEW.md`

---

## 2026-09-04 — CabEnergy elevated to first-class subsystem

**Area:** software / energy / optimization

**Changed:**
- Charging management is a mandatory first-class product, not a minor fleet-management feature.
- CabEnergy will eventually optimize vehicle readiness, site power, energy cost, demand charges, charger allocation, expected dispatch/revenue, and potentially battery/degradation considerations.
- CabEnergy produces provider-neutral desired states; provider adapters issue actual charging commands.

**Reason:**
- At fleet scale, total charging capacity and utility demand can become a binding operational constraint.
- The optimal vehicle to charge is not always simply the vehicle with the lowest SOC.

**Open gates / follow-up:**
- P0.10B must define vehicle, readiness, charging, site-power, plan, command, and verification contracts.
- Future optimization/ML should be developed after deterministic production constraints and simulator contracts are stable.

**Affected docs/code:**
- `docs/phase-0/PHASE_0_OVERVIEW.md`
- `docs/phase-0/P0_10A_TESLA_INTEGRATION_CONTRACT.md`

---

## 2026-09-04 — Tesla integration architecture documented as P0.10A

**Area:** architecture / provider integration / security

**Changed:**
- Defined CabOps as provider-neutral.
- Isolated Tesla functionality behind `TeslaAdapter`.
- Adopted Tesla Fleet Telemetry as the preferred Tesla state-ingestion path when supported and authorized.
- Kept polling as reconciliation/fallback rather than primary state collection.
- Required a durable command request/acknowledgement/verification ledger.
- Defined Tesla for Business / managed-fleet delegation as the preferred customer-management relationship where supported.
- Preserved CabVision independence from Tesla camera access.
- Reserved Cybercab-specific Robotaxi integration behind explicit capability gates rather than assumptions.
- Defined NexusOS -> CabOps -> TeslaAdapter as the integration boundary; NexusOS must not call Tesla directly.
- Identified an ordinary authorized Tesla as a potential `DEV-001` integration mule before Cybercab access.

**Reason:**
- Tesla already exposes significant fleet-management, telemetry, charging, command, service, energy, and business-delegation capabilities.
- Reusing provider capabilities lowers implementation risk while keeping CyberCab's operational intelligence portable.

**Open gates / follow-up:**
- Tesla Cybercab Fleet API parity.
- Cybercab Fleet Telemetry parity.
- Private/commercial Cybercab management delegation.
- Autonomous reposition/depot dispatch.
- Robotaxi network state/control.
- Ride-session and settlement/revenue interfaces.
- Cybercab cabin-camera/occupancy interfaces.
- Charging/depot integration details.
- Intercity autonomous permissions.

**Affected docs/code:**
- `docs/phase-0/P0_10A_TESLA_INTEGRATION_CONTRACT.md`
- `docs/phase-0/PHASE_0_STATUS.md`

---

## 2026-09-04 — Next architecture pass selected

**Area:** architecture

**Changed:**
- Selected **P0.10B — CabOps Core Domain & Event Contracts** as the next architecture pass.

**Reason:**
- Provider-neutral shared domain objects, state machines, events, commands, idempotency, ordering, tenant boundaries, and audit authority should be stable before substantive feature implementation.

**Open gates / follow-up:**
- Define P0.10B and update status after review.

**Affected docs/code:**
- `README.md`
- `docs/phase-0/PHASE_0_STATUS.md`

---

## 2026-09-04 — CabOps core domain/event contract documented as P0.10B

**Area:** architecture / domain model / eventing / tenancy / reliability

**Changed:**
- Defined the core CabOps bounded contexts and their authoritative responsibilities.
- Formalized `CabCare` and `CabIncident` as first-class software domains.
- Separated tenant, owner, manager, payer, provider account, and vehicle identity.
- Adopted effective-dated vehicle management assignments so ownership/management changes preserve historical tenancy boundaries.
- Separated provider/raw signals, canonical observations, domain events, commands, operational plans, projections, and audit records.
- Rejected a single giant vehicle-status enum in favor of multi-dimensional lifecycle/connectivity/presence/dispatch/care/maintenance/energy/readiness state.
- Defined readiness requirements as CabDispatch-owned inputs consumed by CabEnergy/CabCare/CabMaint/CabDepot.
- Required durable operational plans and reason-coded decisions for explainability.
- Defined command lifecycle through request, validation, dispatch, provider acknowledgement, and independent effect confirmation.
- Adopted at-least-once delivery assumptions, idempotent consumers, and transactional outbox/inbox semantics or equivalent guarantees.
- Defined event-time-aware handling of duplicate and out-of-order telemetry.
- Added explicit capability negotiation so core logic asks for capabilities rather than inferring behavior from model names.
- Defined depot visits as containers for parallel jobs/resource reservations rather than one serial mega-state-machine.
- Added shared work-order, inspection-finding, incident, maintenance-restriction, resource-reservation, failure, dead-letter, operator-override, schema-versioning, replay, and audit contracts.
- Required real and simulated providers to use the same canonical contracts.

**Reason:**
- Production fleet operations will combine noisy external telemetry, physical facility state, multiple owners/managers, autonomous decisions, provider commands, human exceptions, and later simulation/ML.
- Without stable authority/event/command semantics, individual modules would become tightly coupled and future provider changes could force core redesign.

**Freeze decision:**
- P0.10B is a **FREEZE CANDIDATE**, not yet fully frozen.

**Implementation proof gates before `FROZEN`:**
- prove out-of-order telemetry cannot regress current state;
- prove duplicate events/commands are harmless under retry;
- prove provider acknowledgement and physical effect are reconciled separately;
- prove ownership/management reassignment does not leak historical tenant data;
- prove simulator and a real provider adapter drive the same workflows;
- prove charging and cleaning can overlap within one depot visit;
- prove blocking incidents/maintenance restrictions reliably remove readiness;
- prove deterministic projection replay under the selected persistence model.

**Open gates / follow-up:**
- Continue to **P0.10C — CabEnergy Deterministic Constraint & Scheduling Contract**.
- Do not select optimization/ML techniques until the deterministic feasible scheduling problem is defined.

**Affected docs/code:**
- `docs/phase-0/P0_10B_CABOPS_CORE_DOMAIN_EVENT_CONTRACTS.md`
- `docs/phase-0/PHASE_0_STATUS.md`
- `README.md`

---

## 2026-09-04 — Sacramento startup cost and property model documented as P0.20A

**Area:** facility / real estate / startup capital / washwater / charging / compliance

**Changed:**
- Completed a current Sacramento-area physical startup cost and property deep dive using live September 2026 industrial listings and current regulatory/vendor benchmarks.
- Formalized seven property archetypes from yard-only IOS through mature existing fleet-service facilities.
- Identified a roughly **10,000–15,000 SF fenced paved/concrete industrial yard** as a viable first physical pilot archetype when zoning/use, washwater, water, power, landlord, access, and insurance gates pass.
- Documented a current Rancho Cordova IOS asking-rate benchmark of approximately **$0.16–$0.22/SF/month**.
- Added current building+yard Sacramento references at approximately $3,450/month, $7,185/month, and $15,000/month depending scale and facilities.
- Documented that Sacramento County recognizes portable washwater-containment/capture methods, allowing a compliant startup path without immediately constructing a permanent wash rack.
- Added current commercial wash/detail equipment planning costs and warned that a newly constructed permanent industrial wash bay can become a six-figure project.
- Added Tesla commercial Level 2 installed-cost guidance and current SMUD commercial EV incentives.
- Added a hard pre-lease requirement to evaluate electrical capacity, including SMUD Grid Capacity Evaluation for serious SMUD-territory candidates.
- Added California car-wash registration / bond planning inputs, local business-license references, and garagekeepers/insurance planning requirements.
- Added startup cash scenarios for yard-only, small building+yard, scalable depot, and permanent-wash cases.
- Adopted **lease first** as the startup real-estate strategy; bare-land development is not the default first-depot path.
- Added a 100-point property scoring model and automatic rejection gates.

**Reason:**
- Rent alone does not determine depot economics. Power, wastewater, zoning, insurance, and avoided tenant improvements can dominate the true cost of a site.
- A paved yard lets the company validate CabOps, CabEnergy, cleaning, inspection, customer custody, and fleet flow at much lower fixed cost than immediately leasing/building a mature hub.

**Current planning conclusion:**
- A properly reserved lean yard-only pilot is modeled around **roughly $50,000–$80,000 available startup cash** if major electrical, sewer, or civil construction is not required.
- A small building + useful yard moves the practical target toward roughly **$85,000–$125,000** under similarly favorable infrastructure conditions.
- A 0.75–1.25 acre scalable depot can require roughly **$134,000–$335,000** before a new permanent wash bay.
- A newly constructed permanent wash bay can add approximately **$85,000–$200,000+** based on current industrial construction benchmarks, so it is not part of the preferred startup architecture.

**Open gates / follow-up:**
- Continue physical workstream with **P0.20B — Sacramento Property Screening & Utility Preflight**.
- Obtain broker quotes for current paved-yard candidates.
- Confirm land use, washwater/sewer path, landlord improvement rights, and actual utility capacity address by address.
- Obtain garagekeepers/garage liability quotes based on 10/25/50/100 customer vehicles.
- Obtain commercial EVSE ROM quotes and SMUD capacity evaluations for serious candidates.
- Build 10/25/50 vehicle layouts against actual parcel geometry.

**Affected docs/code:**
- `docs/phase-0/P0_20A_SACRAMENTO_STARTUP_COST_PROPERTY_MODEL.md`
- `docs/phase-0/PHASE_0_STATUS.md`
- `README.md`
