# Phase 0 Overview — CyberCab Detail + Cab

**Status:** ACTIVE DESIGN  
**Primary geography:** Sacramento, California  
**Long-term scope:** provider-neutral autonomous-fleet operations and infrastructure  
**Documented:** 2026-09-04

Phase 0 defines the true future production business and architecture first, then identifies the smallest pilot that can validate the system without forcing later redesign.

The project is not being optimized for the fastest possible MVP. It is being optimized for a **right first architecture** that can scale from a small Sacramento depot to a multi-node autonomous fleet network.

---

## P0.1 — Business definition: passive robotaxi ownership

The primary customer is not the rider. The primary managed-fleet customer is the asset owner who wants robotaxi income without doing the daily work required to keep the asset productive.

Core customer promise:

> **You own the fleet. We operate the fleet.**

Working service layers:

### CabCare

- scheduled cleaning
- interior reset
- exterior wash
- emergency spill/bodily-fluid cleanup
- lost-item handling support
- damage documentation

### CabDepot

- secure parking/storage
- cleaning
- charging
- inspection
- vehicle readiness
- basic operational checks

### CabOps

- complete fleet-management orchestration
- charging/readiness planning
- maintenance/work-order coordination
- incident handling
- owner reporting
- revenue/cost visibility
- depot/node routing
- availability management

### Owned Fleet

- company-owned autonomous vehicles use the same physical/software infrastructure
- excess cash can be allocated into an acquisition reserve
- customer management revenue helps fund infrastructure that also lowers the marginal operating cost of the owned fleet

---

## P0.2 — Sacramento as Hub 001

Sacramento is the intended first full-service operating hub.

Primary candidate regions include industrial areas with strong power, circulation, sewer/washwater feasibility, and freeway access. Exact site selection remains property-specific and must be validated against real utility capacity and zoning.

Key location principle:

> **Select the property based on operations and electrical capacity before aesthetics.**

Sacramento is preferred over beginning in the Bay Area because the business can target a lower-cost operational center while still sitting near major Northern California corridors.

---

## P0.3 — Regional network model

The long-term system should not assume every vehicle must return to one home depot.

Site hierarchy:

### Level 1 — Hub

Full service:

- cleaning
- charging
- inspection
- maintenance support
- storage
- staffing
- deep clean / exception handling

### Level 2 — Depot

- cleaning
- charging
- inspection
- limited storage

### Level 3 — Node

- charging
- emergency service
- light inspection / reset

### Level 4 — Partner node

An existing qualified business operates to our standards and integrates with CabOps rather than One Village building every physical location itself.

Potential future corridor concepts, subject to autonomous-service permissions and economics:

- Sacramento -> Davis -> Vacaville -> Fairfield -> Bay Area edge
- Sacramento -> Elk Grove -> Stockton -> Modesto
- Sacramento -> Roseville/Rocklin -> Auburn
- later Sacramento -> Auburn -> Truckee/Reno if legally and operationally viable

The network itself can become a competitive moat by reducing deadhead miles and increasing vehicle utilization.

---

## P0.4 — Manual-first wash architecture

Do not buy a large automatic wash system simply because one is available at a relatively low machine price.

Initial wash system should be a covered drive-through/manual bay with provisions such as:

- overhead pressure-washer boom
- foam application
- spot-free/DI rinse where justified
- vacuum/air system
- interior cleaning carts
- excellent lighting
- floor containment
- wastewater capture/recovery
- sewer/reclamation compliance
- camera coverage

The facility should reserve an **automatic wash equipment envelope** so a future touchless/rollover system can be installed without rebuilding the entire site.

Automation is purchased only after the operating data shows that manual wash throughput is a real bottleneck and the payback is acceptable.

---

## P0.5 — Vehicle-flow facility design

The depot should behave more like a production line than a conventional parking lot.

Target flow:

```text
ARRIVAL
  |
  v
Vehicle identification / telemetry ingest
  |
  v
Inspection portal
  |
  +--> normal vehicle --------+
  |                           |
  +--> exception vehicle --> Exception Bay
                              |
                              v
                         Wash / Interior
                              |
                              v
                         Maintenance gate
                              |
                              v
                        Charging / Storage
                              |
                              v
                           READY
                              |
                              v
                           DISPATCH
```

Most cars should follow a predictable fast path. Human time should concentrate on exceptions.

---

## P0.6 — Exception-based cleaning and inspection

The operating target is not a traditional full detail on every car after every shift.

Routine vehicles receive a standardized reset. Problem vehicles receive targeted intervention.

CabVision should eventually classify conditions such as:

- trash
- wrappers / bottles / cups
- visible stains
- liquid spills
- suspected bodily-fluid events
- abandoned property
- torn upholstery
- broken interior parts
- exterior damage
- glass damage
- wheel/tire anomalies

Example result:

```text
CC-0184
cleaning_exception: true
probable_liquid_spill: rear passenger floor
lost_item: possible backpack
new_exterior_damage: passenger rear panel
confidence: 0.96
```

The human operator then receives an explicit work queue rather than manually deciding the full workflow for every returning vehicle.

---

## P0.7 — CabEnergy is mandatory

Charging management is a first-class operating system, not a convenience feature.

Every managed vehicle may eventually expose or derive:

- SOC
- estimated usable range
- target readiness time
- required target SOC
- charging curve / current charging rate
- charger assignment
- vehicle operational state
- maintenance state
- next predicted dispatch window
- expected revenue opportunity

CabEnergy's question is not merely:

> Which vehicle has the lowest battery?

It is:

> Which vehicle should receive the next available unit of energy to maximize fleet readiness and economics under site constraints?

---

## P0.8 — Site-level energy optimization

At larger scale, CabEnergy must optimize the entire depot, not individual vehicles independently.

Inputs may include:

- fleet readiness requirements
- charger availability
- transformer/service capacity
- site power cap
- electricity tariff
- demand charges
- predicted dispatch
- charger power limits
- stationary battery/solar state if present
- battery/degradation constraints if supported by evidence

Conceptual objective:

```text
Fleet Profit
= Ride / operating revenue
- electricity
- demand charges
- maintenance
- degradation-related costs
- downtime
- management / operating overhead
```

The optimizer should avoid unnecessary peak kW while still keeping the right vehicles ready at the right times.

This is expected to become a legitimate deterministic optimization and later ML/forecasting problem.

---

## P0.9 — Provider-neutral CabOps

CabOps should work as a standalone fleet-operations platform and also integrate natively with NexusOS.

Working modules:

| Module | Responsibility |
|---|---|
| Fleet Registry | vehicle, owner, contract, and provider identity |
| CabEnergy | charging and site-energy optimization |
| CabVision | inspection and computer vision |
| CabCare | cleaning workflows |
| CabMaint | maintenance and work orders |
| CabIncident | spills, damage, lost property, exceptions |
| CabRoute | depot/node routing and deadhead optimization |
| CabRevenue | owner statements and unit economics |
| CabDispatch | readiness / operating state |
| CabDepot | bays, chargers, positions, physical capacity |
| CabOwner | customer/investor portal |

NexusOS sits above CabOps and should never depend directly on a vehicle manufacturer API.

---

## P0.10 — Tesla integration strategy

Tesla is the first provider integration, not the architecture itself.

Core decisions are documented in:

[`P0_10A_TESLA_INTEGRATION_CONTRACT.md`](P0_10A_TESLA_INTEGRATION_CONTRACT.md)

Key rules:

- CabOps remains provider-neutral.
- Tesla-specific behavior lives in `TeslaAdapter`.
- Fleet Telemetry is the preferred state source where supported.
- Polling is reconciliation/fallback.
- Commands are durable and independently verified.
- Tesla camera access is optional enrichment, never a CabVision dependency.
- Cybercab-specific Robotaxi functions remain explicit capability gates until documented/validated.
- NexusOS -> CabOps -> TeslaAdapter is the allowed integration direction.

---

## P0.11 — Privacy / camera architecture

CabVision should use a minimal-retention model.

Preferred pipeline:

```text
raw image / frame
      |
      v
local or controlled inference
      |
      v
structured finding
      |
      +--> no incident -> delete according to short retention policy
      |
      +--> incident -> retain minimal evidence under incident policy
```

Prefer durable records such as:

```text
trash_detected = true
spill_detected = false
lost_item_detected = true
confidence = 0.982
```

over indefinite passenger-video storage.

---

## P0.12 — CabRoute and network moat

CabRoute should eventually determine whether a vehicle should:

- return to its home hub
- use a closer node
- charge locally
- receive a clean/inspection before returning to service
- deadhead to another demand area
- remain parked because dispatch economics are unfavorable

Inputs may include:

- SOC
- service readiness
- predicted demand
- distance to sites
- site capacity
- current cleaning/maintenance state
- charging availability
- local demand/revenue forecast

The value proposition becomes stronger than simple vehicle storage:

> **Your vehicle has access to an operating network, not just a parking spot.**

---

## P0.13 — Intercity operation is capability-gated

CabOps should support geographic scopes such as:

```text
LOCAL_ONLY
MULTI_ZONE
INTERCITY
```

without assuming Tesla or regulators currently allow each one.

Software should be ready to turn capabilities on when legally and technically available rather than require a rewrite later.

---

## P0.14 — Owner economics simulator

Before final pricing or major property commitments, build a Cab Economics Simulator.

Potential inputs:

- acquisition cost
- financing
- insurance
- rides / operating hours
- revenue per mile or hour
- paid vs deadhead miles
- electricity
- cleaning
- tires
- maintenance
- depreciation
- downtime
- management fee
- utilization

Outputs:

- gross revenue
- operating expenses
- EBITDA / contribution per vehicle
- owner distribution
- our management revenue
- our gross margin
- vehicle payback
- fleet IRR / return scenarios

Use downside/base/upside cases rather than a single optimistic number.

---

## P0.15 — Pricing architecture

Pricing remains a validation item, not a frozen assumption.

Candidate structures include:

### CabCare

Per-event or low recurring subscription.

### CabDepot

Fixed recurring fee for storage, normal cleaning, inspection, and charging management, with electricity metered/pass-through or transparently marked up.

### CabOps

Base recurring fee plus a percentage of managed revenue may align incentives better than a pure flat fee.

### Activation / operating reserve

Customer onboarding may include an activation fee or reserve to cover baseline inspection, hardware/tagging, configuration, initial maintenance, and working-capital exposure.

---

## P0.16 — Customer discovery

Initial customer discovery should test whether the original owner profile generalizes.

Questions should include:

- How many vehicles would the customer consider owning?
- What monthly profit would trigger another purchase?
- Which tasks do they refuse to personally handle?
- Flat monthly fee vs revenue percentage preference?
- Would they delegate maintenance, insurance documentation, lost property, and repairs?
- What level of reporting do they expect?
- What would make them trust a manager with a high-value asset?

The target is not generic enthusiasm. It is evidence of willingness to sign an LOI, reservation, or management agreement.

---

## P0.17 — Facility de-risking rule

Do not sign a large depot lease purely on speculative future demand.

A future gate should require enough committed vehicles / contracted recurring revenue to cover a meaningful share of unavoidable fixed costs.

The exact threshold remains to be modeled.

Principle:

> **Customers should pull the facility into existence.**

---

## P0.18 — Equipment acquisition ladder

Working progression:

### 0–10 managed vehicles

- portable/manual cleaning equipment
- minimal fixed equipment
- no automatic wash

### 10–25

- permanent covered/manual wash bay
- overhead booms
- better vacuum/air systems
- inspection instrumentation

### 25–50

- additional interior capacity
- improved water recovery
- enhanced automated inspection
- expanded charging bank

### 50+ or economic trigger

- evaluate touchless/rollover automation based on actual throughput and payback

### 100+

- increasingly automated flow from arrival -> inspection -> wash -> interior -> charging

### 250+

- parallel lanes and exception-focused human staffing

Vehicle count is guidance only. The actual purchase gate is based on bottleneck and economics.

---

## P0.19 — Owned-fleet acquisition flywheel

Working capital allocation concept:

```text
Operating expenses
      |
Emergency reserve
      |
Equipment / capacity reserve
      |
Debt obligations
      |
Growth reserve
      |
Cybercab / AV acquisition fund
      |
BUY VEHICLE
```

The intent is to compound productive assets rather than increase lifestyle overhead as the service business grows.

This is a strategic principle, not yet a final treasury policy.

---

## P0.20 — Physical Phase 0 deliverables

Phase 0 should eventually produce:

- Sacramento property database
- electrical-capacity checklist
- utility / SMUD process notes
- washwater / sewer requirements
- zoning matrix
- equipment/vendor matrix
- insurance analysis
- labor model
- facility layout(s)
- security requirements
- capex / opex model
- 10 / 25 / 50 / 100 / 250-vehicle facility scenarios

Electrical capacity must be treated as a first-class property attribute.

---

## P0.21 — Software Phase 0 deliverables

Before large-scale feature coding, Phase 0 should produce stable contracts for:

- provider-neutral vehicle identity/state
- tenant/owner/manager identity
- TeslaAdapter
- Fleet Telemetry ingress
- CabEnergy
- CabVision events
- CabRoute node/depot model
- CabMaint/work orders
- CabRevenue/accounting
- incident handling
- maintenance state
- command/audit ledger
- NexusOS integration
- security/privacy
- simulator/digital twin

---

## P0.22 — DEV-001 strategy

An existing authorized Tesla can be used as `DEV-001` to validate much of the real integration stack before owning a Cybercab.

Potential validation targets:

- OAuth
- virtual keys / command architecture
- vehicle discovery
- telemetry ingestion
- SOC / charging state
- location / route state
- charging commands
- maintenance alerts
- command verification
- NexusOS ingestion

The real vehicle can be combined with simulated fleet members to test scale before physical fleet acquisition.

---

## P0.23 — Fleet simulator / digital twin

The software should be able to simulate scenarios such as:

- 100 vehicles
- 20 charging positions
- clustered nighttime arrivals
- mixed SOC distributions
- multiple cleaning exceptions
- maintenance failures
- morning readiness requirements
- site power caps
- demand-charge constraints

The simulator should prove operating contracts before expensive physical scale-up.

---

## P0.24 — Phase 0 gate categories

Phase 0 should track explicit gates in at least these categories:

### Tesla / provider

- private commercial Cybercab purchase
- Fleet API / Telemetry parity
- owner/manager delegation
- autonomous dispatch/reposition
- Robotaxi network interfaces
- ride/revenue data
- camera / occupancy data

### California / regulation

- legal operating model
- autonomous fleet-management permissions
- intercity limitations
- facility/business requirements

### Property

- suitable industrial site
- zoning
- sewer/washwater
- circulation
- security

### Power

- service capacity
- upgrade lead time/cost
- tariff economics
- charger deployment

### Operations

- cleaning minutes per vehicle
- exception rate
- labor productivity
- automated inspection performance

### Customer

- willingness to pay
- LOIs/reservations
- preferred contract structure

### Economics

- managed gross margin at 10 / 25 / 50 / 100 vehicles
- capex payback
- fixed-cost coverage

---

## P0.25 — Phase 0 completion philosophy

Phase 0 is complete only when unknowns are either:

1. resolved with evidence, or
2. converted into explicit external gates with an architecture that remains valid regardless of the outcome.

The goal is not to predict every future Tesla feature. The goal is to prevent future Tesla decisions from forcing us to rebuild our core system.

---

# Current next step

**P0.10B — CabOps Core Domain & Event Contracts**

This pass should define:

- canonical domain objects
- tenant / owner / manager boundaries
- vehicle/depot/charger/incident identities
- state machines
- canonical events
- command envelopes
- telemetry envelopes
- event ordering
- idempotency
- capability negotiation
- audit authority
- retry/failure semantics
- shared APIs between CabEnergy, CabMaint, CabRoute, CabVision, CabDepot, CabRevenue, adapters, and NexusOS
