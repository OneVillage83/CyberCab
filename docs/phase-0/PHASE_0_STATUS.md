# Phase 0 Status — CyberCab Detail + Cab

**Last updated:** 2026-09-04  
**Overall phase:** ACTIVE DESIGN

This file tracks whether each Phase 0 area is concept-only, actively being designed, a freeze candidate, frozen, blocked, or externally gated.

| ID | Area | Status | Notes |
|---|---|---|---|
| P0.1 | Passive-ownership business definition | FREEZE CANDIDATE | Core customer promise and service-layer concept are stable enough for continued planning. |
| P0.2 | Sacramento Hub 001 strategy | ACTIVE DESIGN | Sacramento-first is selected; exact site/utility validation remains open. |
| P0.3 | Regional hub/depot/node network | ACTIVE DESIGN | Corridor concepts exist; intercity autonomy remains externally gated. |
| P0.4 | Manual-first wash architecture | FREEZE CANDIDATE | Start manual/overhead; reserve automatic-wash envelope; automate only on measured bottleneck/payback. |
| P0.5 | Vehicle-flow facility model | ACTIVE DESIGN | Need exact 10/25/50/100/250 vehicle layouts and throughput model. |
| P0.6 | Exception-based cleaning/inspection | ACTIVE DESIGN | Core operating principle accepted; instrumentation/CV contracts remain to be defined. |
| P0.7 | CabEnergy first-class subsystem | FREEZE CANDIDATE | Charging/readiness optimization is mandatory and provider-neutral. |
| P0.8 | Site-level energy optimization | ACTIVE DESIGN | Need deterministic constraint model, SMUD tariff model, and simulation. |
| P0.9 | Provider-neutral CabOps modules | FREEZE CANDIDATE | High-level module boundaries accepted; shared domain contracts are next. |
| P0.10A | Tesla Integration Contract | FREEZE CANDIDATE | Core adapter/telemetry/command architecture is stable; Cybercab-specific capabilities are explicit gates. |
| P0.10B | CabOps Core Domain & Event Contracts | NEXT | Define canonical production objects, state machines, events, commands, idempotency, tenancy, audit, and failure semantics. |
| P0.11 | CabVision privacy architecture | ACTIVE DESIGN | Minimal-retention principle accepted; exact evidence/retention policies remain open. |
| P0.12 | CabRoute / network optimization | ACTIVE DESIGN | Provider-neutral route/node logic planned; autonomous reposition is Cybercab/provider gated. |
| P0.13 | Intercity capability model | EXTERNALLY GATED | Software should support LOCAL_ONLY / MULTI_ZONE / INTERCITY without assuming availability. |
| P0.14 | Owner economics simulator | NOT STARTED | Required before final pricing and large property commitments. |
| P0.15 | Pricing architecture | ACTIVE DESIGN | Candidate fixed/revenue-share/hybrid structures exist; must be validated. |
| P0.16 | Customer discovery | NOT STARTED | Garrett profile is hypothesis seed; need broader interviews/LOIs. |
| P0.17 | Facility de-risking gate | ACTIVE DESIGN | Principle accepted; exact committed-vehicle / MRR threshold not yet modeled. |
| P0.18 | Equipment acquisition ladder | FREEZE CANDIDATE | Stage-based acquisition concept accepted; exact economic triggers remain to be calculated. |
| P0.19 | Owned-fleet acquisition flywheel | ACTIVE DESIGN | Strategic principle accepted; treasury policy and reserve thresholds not frozen. |
| P0.20 | Physical Phase 0 deliverables | NOT STARTED | Property database, utility checklist, zoning, vendor, insurance, labor, layout, capex/opex remain. |
| P0.21 | Software Phase 0 deliverables | ACTIVE DESIGN | P0.10A complete; P0.10B next. |
| P0.22 | DEV-001 Tesla integration mule | PLANNED | Existing authorized Tesla can validate Tesla integration before Cybercab acquisition. |
| P0.23 | Fleet simulator / digital twin | PLANNED | Should support mixed real/simulated fleet and depot/energy scenarios. |
| P0.24 | Phase 0 gate register | ACTIVE DESIGN | Tesla/Cybercab gates formalized in P0.10A; regulatory/property/power/customer gates need dedicated docs. |
| P0.25 | Phase 0 completion criteria | FREEZE CANDIDATE | Unknowns must be resolved or isolated as explicit external gates without breaking core architecture. |

---

## Current architecture freeze candidates

- Passive robotaxi ownership as the business thesis.
- Sacramento-first launch geography.
- Manual-first wash with an automatic-wash-ready facility envelope.
- CabEnergy as a provider-neutral first-class subsystem.
- CabOps as the provider-neutral fleet operations core.
- Tesla isolated behind `TeslaAdapter`.
- Fleet Telemetry preferred over repeated polling when supported.
- Durable command request/acknowledgement/verification ledger.
- CabVision remains functional without Tesla camera access.
- NexusOS integrates through CabOps rather than directly with Tesla.
- Cybercab-specific Robotaxi behavior remains behind explicit capability gates.

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

**P0.10B — CabOps Core Domain & Event Contracts**

No major production feature implementation should get ahead of the shared domain/event/command contracts unless the implementation is explicitly disposable experimentation.
