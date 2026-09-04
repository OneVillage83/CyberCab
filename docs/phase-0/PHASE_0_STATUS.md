# Phase 0 Status — CyberCab Detail + Cab

**Last updated:** 2026-09-04  
**Overall phase:** ACTIVE DESIGN

This file tracks whether each Phase 0 area is concept-only, actively being designed, a freeze candidate, frozen, blocked, or externally gated.

| ID | Area | Status | Notes |
|---|---|---|---|
| P0.1 | Passive-ownership business definition | FREEZE CANDIDATE | Core customer promise and service-layer concept are stable enough for continued planning. |
| P0.2 | Sacramento Hub 001 strategy | ACTIVE DESIGN | Sacramento-first is selected; P0.20A establishes current candidate/property archetypes, but address-specific land-use, sewer and power validation remains open. |
| P0.3 | Regional hub/depot/node network | ACTIVE DESIGN | Corridor concepts exist; intercity autonomy remains externally gated. |
| P0.4 | Manual-first wash architecture | FREEZE CANDIDATE | Start manual/portable/overhead; reserve automatic-wash envelope; automate only on measured bottleneck/payback. P0.20A confirms Sacramento County recognizes portable compliant washwater-capture alternatives. |
| P0.5 | Vehicle-flow facility model | ACTIVE DESIGN | P0.20A adds gross yard-capacity assumptions; exact 10/25/50/100/250 vehicle site layouts remain open. |
| P0.6 | Exception-based cleaning/inspection | ACTIVE DESIGN | Core operating principle accepted; CabCare/CabIncident/CabVision domain boundaries are formalized in P0.10B. |
| P0.7 | CabEnergy first-class subsystem | FREEZE CANDIDATE | Charging/readiness optimization is mandatory and provider-neutral. |
| P0.8 | Site-level energy optimization | ACTIVE DESIGN | Need deterministic constraint model, SMUD tariff model, and simulation. P0.20A adds charger cost/incentive/GCE preflight constraints. |
| P0.9 | Provider-neutral CabOps modules | FREEZE CANDIDATE | Shared bounded contexts and core contracts are formalized in P0.10B, including CabCare and CabIncident as first-class domains. |
| P0.10A | Tesla Integration Contract | FREEZE CANDIDATE | Core adapter/telemetry/command architecture is stable; Cybercab-specific capabilities are explicit gates. |
| P0.10B | CabOps Core Domain & Event Contracts | FREEZE CANDIDATE | Canonical identities, observations, events, commands, plans, tenancy, readiness, idempotency, ordering, audit, failure semantics, simulator parity, and domain boundaries are defined; implementation proof gates remain. |
| P0.10C | CabEnergy Deterministic Constraint & Scheduling Contract | NEXT — SOFTWARE | Define the feasible optimization problem, decision variables, constraints, objective terms, replanning triggers, and deterministic baseline solver before ML. |
| P0.11 | CabVision privacy architecture | ACTIVE DESIGN | Minimal-retention principle accepted; exact evidence/retention policies remain open. |
| P0.12 | CabRoute / network optimization | ACTIVE DESIGN | Provider-neutral route/node logic planned; autonomous reposition is Cybercab/provider gated. |
| P0.13 | Intercity capability model | EXTERNALLY GATED | Software should support LOCAL_ONLY / MULTI_ZONE / INTERCITY without assuming availability. |
| P0.14 | Owner economics simulator | NOT STARTED | Required before final pricing and large property commitments. P0.20A supplies initial property/capex inputs. |
| P0.15 | Pricing architecture | ACTIVE DESIGN | Candidate fixed/revenue-share/hybrid structures exist; must be validated. |
| P0.16 | Customer discovery | NOT STARTED | Garrett profile is hypothesis seed; need broader interviews/LOIs. |
| P0.17 | Facility de-risking gate | ACTIVE DESIGN | Principle accepted; P0.20A reinforces lease-first / demand-pulled facility strategy; exact committed-vehicle/MRR threshold remains open. |
| P0.18 | Equipment acquisition ladder | FREEZE CANDIDATE | Stage-based acquisition concept accepted; P0.20A adds current manual equipment and permanent wash-bay cost baselines. |
| P0.19 | Owned-fleet acquisition flywheel | ACTIVE DESIGN | Strategic principle accepted; treasury policy and reserve thresholds not frozen. |
| P0.20 | Physical Phase 0 deliverables | ACTIVE DESIGN | P0.20A starts the physical workstream with current Sacramento property, washwater, charging, compliance and startup-cost research. |
| P0.20A | Sacramento Startup Cost & Property Model | RESEARCH BASELINE | Current listings and cost ranges documented. Paved/concrete yard-only pilot is a freeze candidate; broker/utility/sewer/land-use/insurance quotes remain required before site selection. |
| P0.20B | Sacramento Property Screening & Utility Preflight | NEXT — PHYSICAL | Convert the P0.20A shortlist into address-specific zoning, sewer, water, SMUD GCE, landlord-improvement, insurance and broker-quote evidence. |
| P0.21 | Software Phase 0 deliverables | ACTIVE DESIGN | P0.10A and P0.10B are freeze candidates; P0.10C is next software pass. |
| P0.22 | DEV-001 Tesla integration mule | PLANNED | Existing authorized Tesla can validate Tesla integration before Cybercab acquisition. |
| P0.23 | Fleet simulator / digital twin | PLANNED | Must use the same observation/event/command contracts as production and support mixed real/simulated fleets. |
| P0.24 | Phase 0 gate register | ACTIVE DESIGN | Tesla/Cybercab gates formalized in P0.10A; P0.10B adds implementation proof gates; P0.20A adds explicit property/power/washwater/insurance gates. |
| P0.25 | Phase 0 completion criteria | FREEZE CANDIDATE | Unknowns must be resolved or isolated as explicit external gates without breaking core architecture. |

---

## Current architecture / operating freeze candidates

- Passive robotaxi ownership as the business thesis.
- Sacramento-first launch geography.
- A **10,000–15,000 SF fenced paved/concrete industrial yard can be sufficient for the first physical pilot** when land-use, washwater, water, power, access and insurance gates are satisfied.
- Lease first; do not buy/develop bare land for the startup depot by default.
- Use portable compliant washwater containment first; a newly constructed permanent wash bay is not a startup requirement.
- Manual-first wash with an automatic-wash-ready facility envelope.
- CabEnergy as a provider-neutral first-class subsystem.
- CabOps as the provider-neutral fleet operations core.
- CabCare and CabIncident as first-class bounded contexts rather than hidden CabVision/CabMaint logic.
- Tesla isolated behind `TeslaAdapter`.
- Fleet Telemetry preferred over repeated polling when supported.
- Provider observations, domain events, commands, plans, projections, and audit records are distinct durable concepts.
- Tenant, owner, manager, payer, provider account, and vehicle identities are separate and history preserving.
- Vehicle state is multi-dimensional; overall readiness is derived with reason codes.
- Durable command request/acknowledgement/effect-verification lifecycle.
- At-least-once delivery with idempotent consumers and transactional outbox/inbox semantics or equivalent guarantees.
- Event-time-aware handling of duplicated/out-of-order telemetry.
- Capability negotiation rather than model-name assumptions.
- Depot visits own parallel jobs/resource reservations instead of one giant serial state machine.
- CabVision remains functional without Tesla camera access.
- Simulator and real providers use the same core contracts.
- NexusOS integrates through CabOps rather than directly with Tesla.
- Cybercab-specific Robotaxi behavior remains behind explicit capability gates.

## P0.10B implementation proof gates

Before P0.10B can move from `FREEZE CANDIDATE` to `FROZEN`, implementation/prototype work should prove:

1. out-of-order telemetry cannot regress projected current state;
2. duplicate events/commands are harmless under retry;
3. provider acknowledgement and physical effect are reconciled independently;
4. vehicle ownership/management reassignment does not leak historical tenant data;
5. a real provider adapter and simulator can drive the same core workflow;
6. charging and cleaning can occur within one depot visit without state-machine conflict;
7. blocking incident/maintenance restrictions reliably remove dispatch readiness;
8. projection replay is deterministic under the selected durable history model.

## P0.20A physical-site findings

See [`P0_20A_SACRAMENTO_STARTUP_COST_PROPERTY_MODEL.md`](P0_20A_SACRAMENTO_STARTUP_COST_PROPERTY_MODEL.md).

Key findings:

- current Rancho Cordova IOS yard benchmark: roughly `$0.16–$0.22/SF/month` at 3667 Omec Park Dr;
- current Sacramento Depot Park has fenced paved yard options around 12,115–33,205 SF with broker pricing;
- current 3453 Ramona example: 5,283 SF building + 10,680 SF yard at $7,185/month;
- current 3394 Sunrise example: 6,000 SF warehouse + 34,000 SF concrete yard at $15,000/month + NNN;
- current 7011 Power Inn example: 5,000 SF building + ~2-acre partially paved/fenced yard at $15,000/month NNN;
- current 7080 Florin Perkins mature reference includes a wash rack/oil-water separator, 600A 480V three-phase power and a 372,002 SF paved secured yard;
- portable compliant washwater capture can defer a six-figure permanent wash-bay build;
- SMUD Grid Capacity Evaluation should precede any long-term lease dependent on fleet charging;
- a properly reserved lean yard-only startup is currently modeled around roughly $50k–$80k available cash when major utility/civil work is unnecessary.

## Current external Tesla/Cybercab gates

See [`P0_10A_TESLA_INTEGRATION_CONTRACT.md`](P0_10A_TESLA_INTEGRATION_CONTRACT.md) for the authoritative register.

High-priority open items include:

- Cybercab Fleet API support/parity
- Cybercab Fleet Telemetry support/parity
- commercial/business vehicle-manager delegation
- autonomous reposition / depot dispatch
- Robotaxi network entry/exit/control
- ride-session data
- revenue / settlement data
- cabin-camera / occupancy data
- charging/depot interface details
- service integration parity
- intercity autonomous operation
- private-owner network participation terms

## Next work

Two parallel next passes are now valid:

### Software

**P0.10C — CabEnergy Deterministic Constraint & Scheduling Contract**

Define the deterministic feasible scheduling problem before choosing optimization libraries or adding ML/forecasting.

### Physical / business infrastructure

**P0.20B — Sacramento Property Screening & Utility Preflight**

Turn current candidate properties into address-specific evidence: broker quote, exact lease terms, zoning/use confirmation, washwater/sewer path, electrical service, SMUD Grid Capacity Evaluation, EVSE buildout rights, and insurance feasibility.
