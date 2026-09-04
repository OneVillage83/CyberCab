# CyberCab

CyberCab is the working repository for **CyberCab Detail + Cab**, a Sacramento-first autonomous-vehicle operations and infrastructure business designed to make robotaxi ownership as passive as possible.

The project is intentionally broader than detailing. The long-term product is a **turnkey robotaxi depot and fleet-operations platform** combining physical infrastructure, recurring fleet services, software, energy optimization, inspection/vision, maintenance orchestration, and eventually an owned autonomous fleet.

## Core thesis

A future robotaxi owner may want the income from one or more autonomous vehicles without wanting to personally clean, charge, park, inspect, maintain, recover, or manage them. CyberCab Detail + Cab exists to absorb that operating burden.

The business layers are currently modeled as:

1. **CabCare** — cleaning/detailing and emergency cleanup.
2. **CabDepot** — storage, cleaning, inspection, charging, and vehicle readiness.
3. **CabOps** — complete fleet operations and passive-ownership management.
4. **Owned Fleet** — company-owned autonomous vehicles whose profits compound into additional fleet assets.

The long-term physical network is intended to use Sacramento as the first full-service hub, with progressively lighter depots/nodes along important regional corridors as autonomous intercity operation becomes feasible.

## Software architecture

CyberCab's software is designed to be provider-neutral.

```text
NexusOS
   |
   v
CabOps API
   |
   +-- CabEnergy
   +-- CabMaint
   +-- CabRoute
   +-- CabVision
   +-- CabDepot
   +-- CabRevenue
   |
   v
Provider adapters
   +-- TeslaAdapter
   +-- Future AV providers
```

Tesla software and APIs are treated as provider integrations rather than the core architecture. CabOps owns the operational intelligence; NexusOS supplies higher-level orchestration across One Village systems.

## Phase 0 objective

Phase 0 is not a fast-MVP exercise. Its purpose is to define the **production future architecture first**, identify external gates, validate the Sacramento business model, and determine the smallest real-world pilot that teaches us useful production lessons without creating avoidable architectural debt.

Current Phase 0 work includes:

- customer/service model
- Sacramento facility and property strategy
- charging/site-energy architecture
- manual-to-automated wash transition
- Tesla integration contract
- fleet telemetry and command architecture
- Cybercab capability gate tracking
- owner economics and pricing
- operational simulator / digital-twin design
- regional hub/node expansion model
- NexusOS integration boundary

See [`docs/phase-0/PHASE_0_OVERVIEW.md`](docs/phase-0/PHASE_0_OVERVIEW.md).

## Documentation policy

This repository is documentation-first. Significant architecture, code, configuration, facility, business-model, vendor, pricing, regulatory, or operational decisions must be documented when they are made.

Every material change should leave enough evidence that a future contributor can answer:

- What changed?
- Why was it changed?
- What alternatives were rejected?
- What assumptions or external gates remain?
- What is the next expected step?

See [`docs/DOCUMENTATION_POLICY.md`](docs/DOCUMENTATION_POLICY.md) and [`docs/PROJECT_LOG.md`](docs/PROJECT_LOG.md).

## Current status

- **Phase:** P0 — Production architecture and feasibility
- **Primary geography:** Sacramento, California
- **Backup/expansion geography:** surrounding Sacramento region first; Northern California nodes later
- **Tesla integration:** P0.10A documented; core integration architecture is a freeze candidate
- **Next architecture pass:** P0.10B — CabOps Core Domain & Event Contracts

## Repository map

```text
README.md

docs/
  DOCUMENTATION_POLICY.md
  PROJECT_LOG.md
  phase-0/
    PHASE_0_OVERVIEW.md
    PHASE_0_STATUS.md
    P0_10A_TESLA_INTEGRATION_CONTRACT.md
```

---

**Working project name:** CyberCab Detail + Cab  
**Repository:** `OneVillage83/CyberCab`  
**Owner:** One Village LLC / OneVillage83
