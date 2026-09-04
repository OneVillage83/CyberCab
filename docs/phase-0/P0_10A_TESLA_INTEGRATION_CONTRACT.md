# P0.10A — Tesla Integration Contract v0.1

**Project:** CyberCab Detail + Cab  
**Platform layer:** CabOps / NexusOS  
**Status:** CORE ARCHITECTURE FREEZE CANDIDATE; CYBERCAB CAPABILITY PROFILE OPEN  
**Documented:** 2026-09-04

## Purpose

Define the production boundary between Tesla-provided fleet capabilities and CyberCab Detail + Cab's provider-neutral fleet-operations platform.

The objective is to use Tesla's official fleet infrastructure where it exists without allowing Tesla-specific implementation details or undocumented Cybercab assumptions to become the core architecture.

---

## 1. Primary architecture decision

CyberCab Detail + Cab will **not** build a replacement for Tesla's fleet-management stack.

Instead:

- **CabOps** owns provider-neutral autonomous-fleet operations intelligence.
- **TeslaAdapter** owns Tesla authentication, state, telemetry, commands, charging, service, energy, and future Robotaxi integration.
- **NexusOS** owns higher-level One Village orchestration, cross-system policy, and intelligence.

```text
                         TESLA
        +------------------+------------------+
        |                  |                  |
 Tesla for Business     Fleet API       Fleet Telemetry
        |                  |                  |
        +------------------+------------------+
                           |
                    +------v-------+
                    | TeslaAdapter |
                    +------+-------+
                           |
                    CabOps Core
                           |
     +----------+----------+----------+----------+
     v          v          v          v          v
 CabEnergy  CabMaint   CabRoute   CabVision  CabDepot
     |          |          |          |          |
     +----------+----------+----------+----------+
                           |
                           v
                       CabRevenue
                           |
                           v
                        NexusOS
```

### Architecture rule

```text
NexusOS -> CabOps -> TeslaAdapter -> Tesla
```

NexusOS must not call Tesla directly.

---

## 2. Capability classification

Use these labels throughout Tesla/Cybercab integration documentation:

| Status | Meaning |
|---|---|
| `AVAILABLE` | Tesla publicly documents the capability today. |
| `PARTIAL` | Tesla provides useful pieces but not the complete CabOps requirement. |
| `BUILD` | Capability belongs to our platform. |
| `CYBERCAB-GATED` | Do not assume until Tesla documents or directly validates privately managed Cybercab behavior. |
| `NOT PUBLICLY EXPOSED` | No supported public Fleet API surface has been verified. |
| `UNVERIFIED` | Evidence exists but has not been validated strongly enough for production use. |

External uncertainty must remain visible as a gate, never buried as an assumption.

---

## 3. Tesla authorization and fleet-management relationships

CabOps must support multiple ownership/management relationships.

| Situation | Tesla relationship | CabOps path |
|---|---|---|
| Existing personal Tesla used for development | Individual owner | Third-party OAuth / owner authorization |
| One Village-owned Tesla fleet | Business-owned vehicles | Business / partner authorization |
| Customer fleet managed by CyberCab Detail + Cab | Customer business delegates vehicles to a Vehicle Management Company | Third-Party for Business / delegated access |
| Institutional customer fleet | Owner business delegates assigned VINs/vehicles | Third-Party for Business / delegated access |

### Hard identity rule

Do not assume:

```text
vehicle_owner == API_tenant == vehicle_manager
```

CabOps must model owner, manager, tenant, user, payer, and provider identities separately.

Tesla for Business / Vehicle Management delegation is the preferred managed-customer relationship where available because customers can retain asset ownership while delegating operational management rather than sharing account credentials.

---

## 4. Runtime capability profiles

Never infer functionality solely from a Tesla model name.

Every vehicle receives a runtime capability profile.

Example:

```text
vehicle_id                  CC-0042
provider                    TESLA
model                       CYBERCAB
telemetry                   UNKNOWN
vehicle_commands            UNKNOWN
charging_commands           UNKNOWN
navigation_command          UNKNOWN
autonomous_reposition       UNKNOWN
cabin_camera_api            UNKNOWN
robotaxi_dispatch_api       UNKNOWN
service_data                UNKNOWN
```

`TeslaCapabilityResolver` is responsible for determining what the currently authorized vehicle/provider relationship actually supports.

Provider-specific assumptions outside adapters are prohibited.

---

## 5. Fleet Registry capability map

| Requirement | Classification |
|---|---|
| List accessible vehicles | `AVAILABLE` |
| VIN identity | `AVAILABLE` |
| Vehicle information | `AVAILABLE` |
| Firmware/version information | `AVAILABLE` |
| Detect Fleet Telemetry support/status | `AVAILABLE` |
| Detect command protocol requirements/status | `AVAILABLE` |
| Vehicle specifications | `AVAILABLE` |
| Fleet-owner delegation to a management company | `AVAILABLE` through Tesla business tooling |
| Manage delegated customer vehicles | `AVAILABLE` where authorization relationship supports it |
| Driver sharing/access | `PARTIAL` / permission-dependent |
| Cybercab-specific Fleet API capability profile | `CYBERCAB-GATED` |
| Cybercab-specific commercial ordering workflow | `CYBERCAB-GATED` |

---

## 6. CabEnergy contract

CabEnergy is a provider-neutral first-class subsystem.

Tesla's current fleet interfaces give us a strong starting point for vehicle charging state and commands, but Tesla does not own our scheduling/optimization logic.

### Tesla-side charging capabilities

| Capability | Classification |
|---|---|
| State of charge | `AVAILABLE` |
| Battery level | `AVAILABLE` |
| Charge state / detailed charge state | `AVAILABLE` |
| Charging power | `AVAILABLE` |
| Charging current | `AVAILABLE` |
| Voltage / phases where exposed | `AVAILABLE` |
| Energy added | `AVAILABLE` |
| Charge limit | `AVAILABLE` |
| Time to full | `AVAILABLE` |
| Start charging | `AVAILABLE` |
| Stop charging | `AVAILABLE` |
| Set charging amps | `AVAILABLE` |
| Set charge limit | `AVAILABLE` |
| Charge scheduling | `AVAILABLE` |
| Preconditioning scheduling/control | `AVAILABLE` where supported |
| Charging history | `AVAILABLE` |
| Charging invoices | `AVAILABLE` |
| Business charging price/session data | `AVAILABLE` / account-dependent |
| Site demand-charge optimization | `BUILD` |
| Physical stall assignment | `BUILD` |
| Non-Tesla EVSE control | `BUILD` |
| SMUD tariff optimization | `BUILD` |
| Cybercab-specific wireless/automated charging behavior | `CYBERCAB-GATED` |

### Hard boundary

```text
CabEnergy computes desired state.
Provider adapter executes provider-specific action.
Telemetry verifies resulting state.
```

No Tesla HTTP/API call belongs inside CabEnergy optimization code.

### Provider abstraction

```text
ChargingProvider
    TeslaChargingProvider
    ChargePointProvider
    WallboxProvider
    FutureWirelessProvider
    SimulatorProvider
```

This permits mixed charging infrastructure and future autonomous charging systems.

---

## 7. Fleet Telemetry is the preferred Tesla state source

### Decision

**Use Fleet Telemetry as primary Tesla state ingestion when supported and authorized.**

Repeated polling should be limited to reconciliation, recovery, capability discovery, or data not available through the stream.

```text
Tesla Vehicle
       |
       | Fleet Telemetry push
       v
Telemetry Gateway
       |
       v
Raw Event Bus
       |
       +--> Current State Projector
       +--> CabEnergy
       +--> CabRoute
       +--> CabMaint
       +--> Alert Engine
       +--> Historical Data Lake
```

### Canonical rule

```text
TELEMETRY > POLLING
```

CabOps normally reads its own durable current-state projection rather than waking/polling vehicles on demand.

This architecture also gives us replayable fleet history for analytics and ML.

---

## 8. CabRoute contract

Tesla can provide useful location and navigation information where the appropriate permissions and vehicle support exist.

| Capability | Classification |
|---|---|
| Current GPS/location | `AVAILABLE` |
| Heading | `AVAILABLE` |
| Active destination | `AVAILABLE` |
| Destination coordinates | `AVAILABLE` |
| ETA / route remaining distance | `AVAILABLE` where exposed |
| Route information | `AVAILABLE` where exposed |
| Nearby charging information | `AVAILABLE` |
| Send navigation destination | `AVAILABLE` |
| Send coordinates/waypoints | `AVAILABLE` where supported |
| Determine nearest CyberCab node | `BUILD` |
| Compute deadhead economics | `BUILD` |
| Select optimal depot/node | `BUILD` |
| Demand forecasting | `BUILD` |
| Command an ordinary consumer Tesla to drive itself unmanned to another location | `NOT PUBLICLY EXPOSED` |
| Dispatch privately managed Cybercab unmanned to depot | `CYBERCAB-GATED` |
| Enter/leave Tesla Robotaxi revenue service | `CYBERCAB-GATED` |
| Passenger-demand feed | `NOT PUBLICLY EXPOSED` |
| Robotaxi fare/revenue feed through current public Fleet API | `NOT PUBLICLY EXPOSED` |
| Intercity autonomous fleet movement | `CYBERCAB-GATED` |

### Critical distinction

A navigation command is not an autonomous reposition command.

Reserve an interface such as:

```text
AutonomousDispatchProvider
```

but do not implement a Tesla production adapter until Tesla publishes or directly validates the relevant private/commercial Cybercab contract.

---

## 9. CabMaint contract

Tesla already provides valuable fleet maintenance signals and business service-management workflows.

| Capability | Classification |
|---|---|
| Odometer | `AVAILABLE` |
| Tire pressure / TPMS state | `AVAILABLE` |
| Recent vehicle alerts | `AVAILABLE` |
| Service data | `AVAILABLE` |
| Firmware version | `AVAILABLE` |
| Software update state | `AVAILABLE` |
| OTA scheduling where supported | `AVAILABLE` |
| Service Mode state | `AVAILABLE` |
| Tesla service history/information | `AVAILABLE` / `PARTIAL` depending on interface |
| Schedule/manage service through Tesla for Business UI | `AVAILABLE` |
| Fully automated public-API service booking | `PARTIAL` / not assumed |
| Automated repair-estimate approval through public Fleet API | `PARTIAL` / not assumed |
| Predictive tire replacement | `BUILD` |
| Predictive component failure | `BUILD` |
| Internal work orders | `BUILD` |
| Third-party repair coordination | `BUILD` |
| Parts inventory | `BUILD` |
| Maintenance reserve forecasting | `BUILD` |

### Initial workflow

```text
Tesla alert/service data
        |
        v
CabMaint
        |
        v
Internal case + severity + readiness impact
        |
        v
Tesla for Business or human-assisted service workflow
        |
        v
Outcome persisted in CabMaint
```

If Tesla later exposes additional write APIs, the human step can be reduced without redesigning CabMaint.

---

## 10. CabVision contract

Cybercab is documented as having a cabin camera used by Tesla for occupancy-related functions. That does **not** mean a general-purpose third-party recorded-video interface exists.

| Capability | Classification |
|---|---|
| Cybercab cabin camera exists | `AVAILABLE` |
| Tesla uses cabin camera for occupancy-related functionality | `AVAILABLE` |
| Sentry control on supported vehicles | `AVAILABLE` |
| Tesla authorization model references Live Camera access | `PARTIAL` |
| General third-party cabin-camera streaming endpoint | `NOT PUBLICLY EXPOSED` |
| General recorded-video download endpoint | `NOT PUBLICLY EXPOSED` |
| Public Cybercab occupancy telemetry field | `NOT PUBLICLY EXPOSED` / `UNVERIFIED` |
| Lost-item detection from Tesla cabin imagery | `CYBERCAB-GATED` |
| Trash/spill detection from Tesla cabin imagery | `CYBERCAB-GATED` |
| Damage detection from vehicle cameras | `CYBERCAB-GATED` |
| Depot exterior inspection cameras | `BUILD` |
| Depot interior inspection cameras | `BUILD` |
| CV anomaly detection | `BUILD` |
| Evidence lifecycle and retention | `BUILD` |

### Required fallback hierarchy

```text
CabVision

1. Cybercab-native structured metadata, if exposed
2. Tesla camera snapshot/stream, if explicitly permitted
3. Depot-owned inspection cameras
4. Human exception review
```

### Hard rule

CabVision must remain fully operable without Tesla passenger/cabin-camera access.

---

## 11. CabVision privacy rule

**Images are evidence, not general fleet telemetry.**

Prefer structured persistent state such as:

```text
trash_detected:       true
spill_detected:       false
lost_item_detected:   true
damage_detected:      false
confidence:           0.982
```

over indefinite raw-image/video retention.

When evidence must be preserved, associate it with:

```text
incident_id
capture_time
retention_reason
authorized_roles
retention_deadline
deletion_time
```

Privacy and consent boundaries must be part of the production data contract, especially for passenger-related imagery.

---

## 12. CabDepot contract

Tesla does not own the physical facility model.

CabDepot owns the operational twin of the site:

```text
Hub
Zone
Gate
Lane
InspectionPortal
WashBay
InteriorBay
ExceptionBay
MaintenanceBay
ChargingPosition
ParkingPosition
CameraPortal
OperatorStation
```

Example:

```text
HUB-001 SACRAMENTO

Gate A                 AVAILABLE
Inspection Portal 01   OCCUPIED: CC-018
Wash Bay 01            AVAILABLE
Interior Bay 01        OCCUPIED: CC-006
Interior Bay 02        AVAILABLE
Exception Bay          OCCUPIED: CC-071
Charger C01            CC-044 / 7.2 kW
Charger C02            CC-052 / 11.5 kW
Charger C03            AVAILABLE
```

Tesla commands may participate in facility workflows, but **CabDepot is authoritative for our facility state**.

---

## 13. CabRevenue contract

Tesla gives us useful cost-side charging information, but current public Fleet API documentation does not establish a complete Robotaxi owner-economics interface.

| Capability | Classification |
|---|---|
| Tesla charging history | `AVAILABLE` |
| Charging invoices | `AVAILABLE` |
| Energy/pricing for qualifying business sessions | `AVAILABLE` |
| Vehicle mileage | `AVAILABLE` |
| Self-driving mileage on supported vehicles | `AVAILABLE` |
| Robotaxi completed rides | `NOT PUBLICLY EXPOSED` |
| Fare per ride | `NOT PUBLICLY EXPOSED` |
| Owner Robotaxi gross revenue | `NOT PUBLICLY EXPOSED` |
| Tesla network fee | `NOT PUBLICLY EXPOSED` |
| Robotaxi payout/settlement API | `NOT PUBLICLY EXPOSED` |
| Ride-level revenue data | `NOT PUBLICLY EXPOSED` |
| Our cleaning/storage/maintenance charges | `BUILD` |
| Owner statements | `BUILD` |
| Profit per vehicle | `BUILD` |
| Payback / IRR analytics | `BUILD` |

### Revenue-provider model

```text
RevenueProvider
    TeslaRobotaxiRevenueProvider      FUTURE
    ImportedSettlementProvider
    ManualSettlementProvider
    OtherNetworkProvider
```

The business must remain operational even if Tesla initially provides owner settlements through files/statements rather than an API.

---

## 14. Tesla Energy / site-energy boundary

CabEnergy must distinguish vehicle energy from site energy.

```text
CabEnergy

VehicleEnergy
    SOC
    charge rate
    target readiness
    battery constraints

SiteEnergy
    utility import
    solar
    stationary battery
    EVSE load
    demand limit
    tariff
```

Tesla Energy can be one `SiteEnergyProvider` where Tesla stationary-energy equipment is used.

CabEnergy must also support non-Tesla EVSE, battery storage, solar, demand response, and future V2G/V2X systems.

---

## 15. TeslaAdapter production boundary

```text
TeslaAdapter
|
+-- TeslaAuth
|   +-- IndividualOAuth
|   +-- PartnerAuth
|   +-- BusinessDelegatedAuth
|
+-- TeslaFleetRegistry
+-- TeslaCapabilityResolver
+-- TeslaTelemetryIngress
+-- TeslaVehicleState
+-- TeslaCommandGateway
+-- TeslaCharging
+-- TeslaNavigation
+-- TeslaService
+-- TeslaEnergy
+-- TeslaBusiness
+-- TeslaCamera              PARTIAL
+-- TeslaRobotaxi            RESERVED / FUTURE
```

### Hard rule

`TeslaRobotaxi` is separate from ordinary Tesla Fleet API code.

Do not contaminate verified current Tesla integration code with guessed future Cybercab behavior.

---

## 16. CabOps canonical vehicle state

All providers translate into CabOps-owned vocabulary.

```text
VehicleState

vehicle_id
tenant_id
owner_id
manager_id

provider
provider_vehicle_id
vin
model
capability_profile

connectivity
operational_state
location
heading
speed

soc
charge_state
charging_power_kw
energy_added_kwh
charge_limit
time_to_full

odometer
tire_pressures
alerts
service_state

route_origin
route_destination
route_eta
route_remaining_distance

cleanliness_state
damage_state

assigned_hub
assigned_position

revenue_state
next_required_ready_at

observed_at
provider_timestamp
ingested_at
```

CabEnergy, CabMaint, CabRoute, CabVision, CabDepot, and NexusOS must consume canonical CabOps contracts, not Tesla-specific field names.

---

## 17. Durable command ledger

No subsystem may issue an ephemeral provider command without durable intent and outcome tracking.

```text
CabEnergy / CabRoute / CabDepot Decision
                  |
                  v
            CommandRequested
                  |
                  v
           Command Policy Gate
                  |
                  v
             TeslaAdapter
                  |
                  v
                Tesla
                  |
                  v
          CommandAcknowledged
                  |
                  v
      Telemetry confirms outcome
                  |
                  v
           CommandVerified
```

Minimum command record:

```text
command_id
tenant_id
vehicle_id
requested_action
desired_state
reason
requesting_system
requesting_user
provider
provider_result
requested_at
acknowledged_at
verified_at
failure_reason
```

Example:

```text
command_id:        CMD-982739
vehicle:           CC-018
command:           SET_CHARGE_CURRENT
old_state:         32A
requested_state:   12A
reason:            SITE_DEMAND_CAP
plan_id:           CEP-20260904-221500
provider_result:   ACCEPTED
verified_state:    12A
status:            VERIFIED
```

This ledger becomes critical once CabEnergy or CabRoute controls large fleets automatically.

---

## 18. Security and tenant isolation

### Credential storage

```text
Tesla client secret        -> secret manager
Tesla private command key  -> KMS/HSM or equivalent
OAuth refresh tokens       -> encrypted token vault
```

Never place sensitive credentials in:

- Git
- plaintext logs
- plaintext database fields
- operator dashboards

### Least-privilege model

```text
Customer Fleet A
    sees Customer Fleet A only

Customer Fleet B
    sees Customer Fleet B only

Depot operator
    sees operationally assigned vehicles

Maintenance technician
    sees maintenance-relevant state

CabEnergy service
    receives narrowly scoped charge authority

CabVision
    does not inherit general vehicle-command authority
```

Provider scopes and CabOps permissions should both follow least privilege.

---

## 19. DEV-001 — existing Tesla development vehicle

Before owning a Cybercab, an existing authorized Tesla can serve as:

```text
DEV-001
Platform: Tesla
Purpose: CabOps integration validation
```

DEV-001 can validate:

- Tesla developer application registration
- OAuth
- virtual-key / secure-command architecture
- vehicle discovery
- Fleet Telemetry
- location
- SOC / charging data
- tire/TPMS information
- route state
- charging commands
- charging schedules
- maintenance alerts
- NexusOS event ingestion
- durable command ledger
- depot-arrival simulation
- owner/economics simulator integration

A human-driven arrival may be modeled as a simulated autonomous return to `HUB-001`; the purpose is to validate architecture, not pretend the vehicle is already a robotaxi.

---

## 20. First CabEnergy live experiment

The first real experiment should prove a complete closed loop:

```text
Tesla
  |
  v
SOC / charge telemetry
  |
  v
CabEnergy current state
  |
  v
Readiness requirement
  |
  v
Charging plan
  |
  v
TeslaAdapter command
  |
  v
Fleet Telemetry
  |
  v
Command verification
```

Example test:

```text
READY REQUIREMENT
07:00
Minimum SOC: 80%

CURRENT
01:00
SOC: 37%

PLAN
Charge according to site constraint
Pause/resume if required
Target: 80%
Precondition before departure
Ready by 07:00
```

Scale progression:

```text
1 real vehicle
5 simulated vehicles
50 simulated vehicles
1 real + 99 simulated
10 real vehicles
50 real vehicles
```

This becomes the first fleet digital-twin environment.

---

## 21. Cybercab capability gate register

These must remain open until Tesla publishes or directly validates the commercial/private-fleet behavior.

| Gate | Capability | Status |
|---|---|---|
| CYB-001 | Cybercab Fleet API support/parity | OPEN |
| CYB-002 | Cybercab Fleet Telemetry support/parity | OPEN |
| CYB-003 | Business / Vehicle Manager delegation | OPEN |
| CYB-004 | Autonomous reposition command | OPEN |
| CYB-005 | Robotaxi network state/control | OPEN |
| CYB-006 | Ride-session data | OPEN |
| CYB-007 | Revenue / settlement API | OPEN |
| CYB-008 | Cabin-camera access | OPEN |
| CYB-009 | Occupancy metadata | OPEN |
| CYB-010 | Cybercab charging interface | OPEN |
| CYB-011 | Depot / mobility-hub integration | OPEN |
| CYB-012 | Service integration parity | OPEN |
| CYB-013 | Intercity autonomous operating capability | OPEN |
| CYB-014 | Private-owner Robotaxi network participation terms | OPEN |

These are **external capability gates, not core architectural blockers**.

---

## 22. NexusOS boundary

NexusOS communicates in higher-level operational goals.

Examples:

```text
Which vehicles will miss morning readiness?
Keep HUB-001 below the configured site power cap.
Which vehicles need human intervention?
Which assets are producing below target economics?
```

CabOps resolves those goals into vehicle/depot plans and provider commands.

NexusOS does not need to know Tesla OAuth details, Tesla command names, Fleet Telemetry encoding, provider regions, or command-signing mechanics.

---

## 23. Strategic ownership boundary

### Tesla owns

- vehicle hardware
- autonomous-driving stack
- Tesla telemetry
- Tesla vehicle commands
- Tesla service ecosystem
- Tesla charging ecosystem
- Tesla Robotaxi marketplace/network

### CyberCab Detail + Cab owns

- fleet-owner relationships
- passive-ownership operating model
- depots and service nodes
- cleaning workflows
- damage inspection
- incident workflows
- maintenance orchestration
- site energy optimization
- fleet readiness optimization
- inter-depot routing
- deadhead optimization
- owner accounting
- long-term operational history
- cross-manufacturer abstractions
- operational ML
- NexusOS integration
- owned-fleet compounding strategy

This is the intended moat.

---

## 24. P0.10A implementation order

1. Create provider-neutral CabOps domain contracts.
2. Define TeslaAdapter interfaces and capability registry.
3. Register Tesla developer application.
4. Establish OAuth and secure-key architecture.
5. Connect DEV-001.
6. Stand up Fleet Telemetry ingestion.
7. Build current-state projection.
8. Build immutable/durable command ledger.
9. Implement CabEnergy V0.
10. Implement CabMaint ingestion.
11. Implement CabRoute telemetry.
12. Build Cybercab simulator profile.
13. Run mixed real/simulated fleet tests.
14. Add CabDepot simulator.
15. Add NexusOS API boundary.
16. Add Cybercab-specific Tesla features only when Tesla documents or validates them.

---

## 25. Freeze decision

### Core architecture — `FREEZE CANDIDATE`

Stable enough to build dependent contracts against:

- CabOps is provider-neutral.
- Tesla is an adapter.
- Fleet Telemetry is the preferred Tesla ingestion mechanism.
- Polling is fallback/reconciliation.
- Commands are durable and verified.
- CabEnergy is independent of Tesla.
- CabVision cannot depend on Tesla camera access.
- Tesla Business delegation is the preferred managed-customer relationship where supported.
- NexusOS communicates with CabOps, not Tesla directly.
- Robotaxi-specific functions live behind a separate future contract.

### Tesla Cybercab capability profile — `NOT FREEZEABLE YET`

Still requires direct Tesla documentation/validation for:

- autonomous dispatch/reposition
- Robotaxi network entry/exit/control
- ride-session data
- revenue/settlement data
- camera access
- Fleet API parity
- Fleet Telemetry parity
- business-management delegation
- charging/depot integration
- service parity
- intercity autonomous operation

---

## 26. Next architecture pass

**P0.10B — CabOps Core Domain & Event Contracts**

It must define:

- canonical domain objects
- tenant / owner / manager identity boundaries
- states and state machines
- canonical events
- command envelopes
- telemetry envelopes
- capability negotiation
- event ordering
- idempotency
- audit authority
- failure/retry semantics
- shared APIs between CabEnergy, CabMaint, CabRoute, CabVision, CabDepot, CabRevenue, adapters, and NexusOS

Substantive production feature code should not outrun these contracts unless explicitly marked as disposable experimentation.

---

## 27. Primary official Tesla references

Checked as part of the 2026-09-04 integration pass:

- Tesla Fleet / Tesla for Business — https://www.tesla.com/fleet
- Tesla Fleet support — https://www.tesla.com/support/fleet
- Tesla Fleet API documentation — https://developer.tesla.com/docs/fleet-api
- Authentication overview — https://developer.tesla.com/docs/fleet-api/authentication/overview
- Third-Party for Business tokens — https://developer.tesla.com/docs/fleet-api/authentication/third-party-business-tokens
- Fleet Telemetry — https://developer.tesla.com/docs/fleet-api/fleet-telemetry
- Available telemetry data — https://developer.tesla.com/docs/fleet-api/fleet-telemetry/available-data
- Vehicle endpoints — https://developer.tesla.com/docs/fleet-api/endpoints/vehicle-endpoints
- Vehicle commands — https://developer.tesla.com/docs/fleet-api/endpoints/vehicle-commands
- Charging endpoints — https://developer.tesla.com/docs/fleet-api/endpoints/charging-endpoints
- Tesla Energy endpoints — https://developer.tesla.com/docs/fleet-api/endpoints/energy
- Virtual keys — https://developer.tesla.com/docs/fleet-api/virtual-keys/overview
- Tesla Fleet Telemetry reference implementation — https://github.com/teslamotors/fleet-telemetry
- Cybercab support — https://www.tesla.com/support/robotaxi/cybercab

---

# Change Log

## 2026-09-04 — P0.10A documented

Added the first formal Tesla integration contract.

Key decisions recorded:

- provider-neutral CabOps
- isolated `TeslaAdapter`
- telemetry-first state ingestion
- durable verified commands
- provider-neutral CabEnergy/CabMaint/CabRoute/CabVision/CabDepot/CabRevenue boundaries
- NexusOS -> CabOps integration boundary
- DEV-001 strategy
- explicit Cybercab capability gates
- P0.10B selected as next architecture pass
