# P0.10B — CabOps Core Domain & Event Contracts v0.1

**Project:** CyberCab Detail + Cab  
**Platform layer:** CabOps / NexusOS  
**Status:** CORE CONTRACT FREEZE CANDIDATE  
**Documented:** 2026-09-04  
**Depends on:** `P0_10A_TESLA_INTEGRATION_CONTRACT.md`

## Purpose

Define the provider-neutral production contracts shared by CabEnergy, CabMaint, CabRoute, CabVision, CabCare, CabIncident, CabDepot, CabDispatch, CabRevenue, provider adapters, the fleet simulator, and NexusOS.

P0.10A established the boundary between Tesla and CabOps. P0.10B establishes the internal language of CabOps so that future Tesla, Cybercab, EVSE, depot, and autonomous-vehicle integrations do not force domain redesign.

The goal is not to pick a framework or database yet. The goal is to freeze the meanings, authority boundaries, state transitions, event semantics, command semantics, tenant isolation, replay rules, and failure behavior that implementation must preserve.

---

# 1. Architecture axioms

The following are hard design rules.

1. **CabOps owns canonical fleet state.** Providers supply observations and commands; provider payload shapes are not the core domain.
2. **Observed fact, business event, command, plan, projection, and audit record are different things.** They must not be collapsed into one generic message type.
3. **Current state is a projection, not the source of history.** Durable observations/events remain replayable.
4. **Delivery is assumed at-least-once.** Every consumer must be idempotent. No design may depend on magical exactly-once messaging.
5. **External telemetry may arrive late, duplicated, or out of order.** Ingestion order must not automatically determine truth.
6. **Commands are intents with preconditions and verification.** Provider acknowledgement is not proof that the desired physical state occurred.
7. **Tenant, owner, manager, payer, provider account, and vehicle are separate identities.** Do not infer one from another.
8. **Unknown capability fails closed for safety-relevant actions.** Unsupported/unknown provider behavior is never guessed.
9. **Time is explicit.** Event time, provider observation time, receipt time, effective time, and expiration time are distinct where relevant.
10. **Simulation uses the same contracts as production.** The simulator may replace a provider adapter, but not bypass domain rules.
11. **Provider-specific fields do not leak into core modules.** Tesla names belong inside `TeslaAdapter`; other providers receive their own adapters.
12. **NexusOS consumes CabOps contracts.** NexusOS never becomes a hidden provider adapter.

---

# 2. Bounded contexts

P0.10B formalizes the following CabOps bounded contexts.

| Context | Authoritative responsibility |
|---|---|
| **Fleet Registry** | Canonical vehicle identity, organizations, provider links, management assignments, capability profile references |
| **CabDispatch** | Operational availability, active readiness requirements, provider/network assignment state, service holds |
| **CabEnergy** | Vehicle charging plans, site-energy plans, charger allocation, readiness-energy risk |
| **CabRoute** | Preferred hub/node routing, deadhead decisions, route economics, future autonomous reposition intent |
| **CabDepot** | Physical site/resource model, reservations, occupancy, depot visits, local capacity |
| **CabVision** | Sensor/image-derived findings and confidence; evidence metadata |
| **CabCare** | Cleaning/reset/deep-clean work orders and completion state |
| **CabIncident** | Spill, damage, lost-property, safety, vandalism, collision, contamination, and other exception cases that can coordinate multiple contexts |
| **CabMaint** | Mechanical/service work orders, restrictions, serviceability, maintenance history |
| **CabRevenue** | Revenue/cost facts, settlements, charges, owner statements, unit economics |
| **Provider Adapters** | Translate provider observations/commands into/from CabOps contracts |
| **NexusOS Integration** | Higher-level policy/goals and cross-business orchestration through CabOps API/contracts |

## 2.1 CabCare and CabIncident are first-class software domains

This pass explicitly keeps **CabCare** and **CabIncident** as first-class bounded contexts rather than burying them inside CabVision or CabMaint.

Reason:

- CabVision detects; it should not decide every operational consequence.
- An incident can require cleaning, maintenance, evidence retention, owner notification, vehicle hold, insurance documentation, and accounting at the same time.
- CabCare has its own staffing, SLA, materials, duration, bay assignment, and completion evidence.

Example:

```text
CabVision finding
    |
    v
CabIncident opens CASE-184
    |
    +--> CabCare deep-clean work order
    +--> CabDepot exception-bay reservation
    +--> CabDispatch hold
    +--> CabRevenue cleaning charge candidate
    +--> owner notification policy
```

No single subsystem becomes a god object.

---

# 3. Identity model

## 3.1 Internal IDs are authoritative

Provider IDs and VINs are attributes, never the sole primary identity.

Canonical identities include at minimum:

```text
organization_id
user_id
tenant_id
vehicle_id
vehicle_management_assignment_id
provider_account_id
provider_vehicle_id
hub_id
resource_id
visit_id
work_order_id
incident_id
plan_id
command_id
event_id
observation_id
audit_id
```

Use globally unique, sortable identifiers in implementation. A UUIDv7-compatible strategy is preferred, optionally with human-readable type prefixes such as `veh_`, `cmd_`, `evt_`, `inc_`, and `hub_`.

The exact storage representation is an implementation decision, but identifiers must not be reused.

## 3.2 Vehicle identity

A physical vehicle has one durable CabOps `vehicle_id` even if:

- provider account changes,
- provider vehicle ID changes,
- ownership changes,
- management company changes,
- the vehicle moves between hubs,
- the vehicle is temporarily offline.

VIN may be used as a strong external identifier when available, but must not be used as the only history/tenancy boundary.

## 3.3 Tenant vs owner vs manager

Definitions:

- **Tenant** — logical customer/data-isolation boundary.
- **Owner** — legal/economic asset owner as known to CabOps.
- **Manager** — organization authorized to operate/manage the asset.
- **Payer** — party responsible for a specific provider/service/charging cost.
- **Provider account** — manufacturer/network account through which access is granted.

Never assume these are the same organization.

## 3.4 Effective-dated management assignments

Ownership and management must be history-preserving.

```text
VehicleManagementAssignment

assignment_id
vehicle_id
tenant_id
owner_org_id
manager_org_id
payer_org_id? 
provider_account_id?
effective_from
effective_to?
status
contract_reference?
```

A later customer/manager must not automatically inherit access to prior tenant history merely because the same VIN is reassigned.

Historical operational and financial records remain scoped to the assignment/tenure under which they were created.

---

# 4. Time model

All persisted timestamps are stored in UTC. Facility/customer time zones are presentation/scheduling context, not storage ambiguity.

Canonical timestamp meanings:

| Field | Meaning |
|---|---|
| `occurred_at` | When the real-world/domain event happened |
| `observed_at` | When a sensor/provider says the measured value applied |
| `provider_timestamp` | Provider-native event timestamp, if distinct |
| `received_at` | When CabOps received the message |
| `recorded_at` | When CabOps durably persisted it |
| `effective_from` / `effective_to` | Business relationship/state validity window |
| `not_before` | Command/plan must not execute before this time |
| `expires_at` | Command/requirement/authorization is invalid after this time |

### Rule

Do not update current state using `received_at` alone when a trustworthy `observed_at` exists.

Late observations are preserved for history even when they are too old to replace current projected state.

---

# 5. Five different record classes

CabOps must distinguish these record classes.

## 5.1 Provider/raw signal

Unmodified or minimally wrapped external provider/device payload.

Purpose:

- debugging
- reconciliation
- provider adapter validation
- replay where retention permits

Provider raw payloads are not a stable internal API.

## 5.2 Canonical observation

A normalized measured/observed fact.

Examples:

```text
SOC = 38%
vehicle_location = ...
tire_pressure_front_left = ...
charger_power_kw = 9.6
trash_detected = true
resource_occupancy = occupied
```

Observation does not inherently mean a business transition happened.

## 5.3 Domain event

An immutable statement that a meaningful business/domain transition occurred.

Examples:

```text
VehicleRegistered
VehicleArrivedAtHub
ReadinessRequirementCreated
ChargingPlanActivated
MaintenanceRestrictionApplied
IncidentOpened
CleaningWorkCompleted
VehicleReadinessChanged
ManagementAssignmentEnded
```

Do not turn every telemetry sample into a domain event.

## 5.4 Command

A requested real-world or provider-side action.

Examples:

```text
SetChargeCurrent
SetChargeLimit
StartCharging
StopCharging
PreconditionVehicle
ReserveDepotResource
ReleaseDepotResource
NavigateToLocation
LockVehicle
```

A command may be accepted, rejected, fail, expire, or be acknowledged without producing the desired effect.

## 5.5 Projection/read model

A derived current or analytical view optimized for reads.

Examples:

```text
VehicleCurrentState
FleetReadinessView
HubCurrentState
EnergyCurrentState
OwnerPortfolioView
OpenIncidentQueue
```

Projections are rebuildable and may be eventually consistent.

---

# 6. Canonical observation envelope

Logical contract:

```text
CanonicalObservation

observation_id
schema_version
observation_type

tenant_id?
management_assignment_id?
vehicle_id?
hub_id?
resource_id?

source_type            # provider / depot_sensor / operator / derived
source_name            # TESLA / SIMULATOR / HUB_CAMERA_01 / etc.
source_event_id?

observed_at
provider_timestamp?
received_at
recorded_at

field
value
unit?
quality
confidence?

provider_metadata?
correlation_id?
trace_id?
```

### Quality values

At minimum:

```text
VALID
STALE
ESTIMATED
DEGRADED
INVALID
UNKNOWN
```

A low-confidence CV finding and an authoritative provider SOC reading are not semantically equivalent. Quality/confidence must remain visible.

---

# 7. Canonical domain-event envelope

Logical contract:

```text
DomainEvent

event_id
schema_version
event_type

aggregate_type
aggregate_id
aggregate_sequence

tenant_id?
management_assignment_id?

occurred_at
recorded_at

authority
actor_type
actor_id?

correlation_id
causation_id?
trace_id?

payload
metadata?
```

## 7.1 Aggregate sequence

Internally emitted events for the same aggregate receive a monotonically increasing `aggregate_sequence`.

This provides deterministic ordering for internal state transitions.

Provider telemetry does not magically inherit this ordering; adapters/normalizers must reconcile external event time separately.

## 7.2 Correlation and causation

Example:

```text
ReadinessRequirementCreated
    correlation_id = morning-wave-2026-09-05
        |
        v
ChargingPlanActivated
    causation_id = readiness event
        |
        v
CommandRequested
    causation_id = plan decision
        |
        v
CommandEffectConfirmed
```

This makes operational behavior explainable after the fact.

---

# 8. Source-of-truth / authority matrix

Authority is bounded by field/domain, not by a single universal database row.

| State/fact | Primary authority |
|---|---|
| Vehicle internal identity | Fleet Registry |
| Current management/ownership relationship | Fleet Registry assignment history |
| Provider-reported SOC/location/vehicle telemetry | Provider adapter canonical observation |
| Depot physical resource occupancy | CabDepot + validated local sensors/operator events |
| Cleaning state | CabCare, informed by CabVision/operator evidence |
| CV detection finding | CabVision |
| Incident case state | CabIncident |
| Mechanical serviceability/restriction | CabMaint |
| Active readiness requirement | CabDispatch |
| Charging/site-energy plan | CabEnergy |
| Preferred hub/node route | CabRoute |
| Provider/network ride assignment state | CabDispatch through provider adapter when exposed |
| Financial settlement fact | CabRevenue / imported provider settlement |
| Provider command acknowledgement | Provider adapter |
| Verified physical command effect | Canonical observation + command verifier |

### Conflict rule

Conflicting observations are preserved. Projectors resolve current state using configured authority, freshness, quality, and event-time rules.

Never silently delete contradictory evidence.

---

# 9. Current-state projection model

Avoid one enormous mutable `vehicle.status` field.

Vehicle state is intentionally multi-dimensional.

```text
VehicleCurrentState

identity
access
connectivity
presence
mobility_dispatch
energy
care
maintenance
incident_holds
route
provider_state
readiness
```

## 9.1 Recommended state dimensions

### Vehicle lifecycle

```text
REGISTERED
ACTIVE
SUSPENDED
RETIRED
```

### Connectivity

```text
ONLINE
OFFLINE
DEGRADED
UNKNOWN
```

### Physical presence

```text
OFFSITE
EN_ROUTE_TO_SITE
ONSITE
DEPARTING
UNKNOWN
```

### Dispatch / revenue availability

```text
UNAVAILABLE
AVAILABLE
ASSIGNED
IN_SERVICE
RETURNING
HELD
UNKNOWN
```

Provider-specific states are mapped into these where possible while raw provider state remains separately available.

### Care state

```text
CLEAN
RESET_REQUIRED
DEEP_CLEAN_REQUIRED
QUARANTINED
UNKNOWN
```

### Maintenance state

```text
SERVICEABLE
SERVICEABLE_WITH_RESTRICTION
OUT_OF_SERVICE
UNKNOWN
```

### Energy readiness

```text
READY
AT_RISK
NOT_READY
UNKNOWN
```

### Overall readiness

```text
READY
READY_WITH_WARNINGS
NOT_READY
UNKNOWN
```

Overall readiness is a **derived decision**, not an independently writable truth field.

---

# 10. Vehicle readiness contract

CabDispatch owns readiness requirements. Other domains contribute whether the requirement can be satisfied.

```text
ReadinessRequirement

requirement_id
vehicle_id
management_assignment_id
ready_by
priority

minimum_soc?
target_soc?
required_hub_or_zone?
required_care_state?
maintenance_policy?
required_capabilities?

source
reason
created_at
expires_at?
status
```

Possible statuses:

```text
ACTIVE
SATISFIED
AT_RISK
MISSED
CANCELLED
SUPERSEDED
```

### Important rule

CabEnergy does not invent business readiness deadlines. It consumes `ReadinessRequirement` and creates an energy plan that attempts to satisfy it.

CabCare/CabMaint/CabDepot similarly report constraints and completion; CabDispatch computes fleet availability.

---

# 11. Operational plans and decisions

Optimization/planning results must be durable and explainable.

Generic logical contract:

```text
OperationalPlan

plan_id
plan_type
schema_version
scope_type
scope_id
horizon_start
horizon_end

input_snapshot_id
objective
constraint_set_version
planner_version

status
created_at
activated_at?
superseded_by?

summary
```

Plan status:

```text
DRAFT
VALIDATED
ACTIVE
SUPERSEDED
COMPLETED
FAILED
CANCELLED
```

Individual plan decisions should carry reason codes.

Example:

```text
CabEnergyDecision

vehicle_id = CC-018
charger = C-07
requested_power_kw = 7.2
start = 01:10
reason_codes = [MORNING_READINESS, SITE_POWER_CAP]
```

The fleet should always be able to answer:

> Why did the system tell this vehicle to do this?

---

# 12. Command contract

## 12.1 Command envelope

```text
CommandRequest

command_id
schema_version
command_type

tenant_id
management_assignment_id?
target_type
target_id

requested_by_type
requested_by_id?
requesting_module

correlation_id
causation_id?
plan_id?

parameters
preconditions

not_before?
expires_at?
idempotency_key
safety_class

created_at
```

## 12.2 Command lifecycle

```text
REQUESTED
VALIDATING
REJECTED
QUEUED
DISPATCHING
ACKNOWLEDGED
EFFECT_CONFIRMED
FAILED
EXPIRED
CANCELLED
UNKNOWN_OUTCOME
```

### Hard rule

`ACKNOWLEDGED != EFFECT_CONFIRMED`

Example:

```text
Tesla accepts SET_CHARGE_CURRENT=12A
          |
          v
Command ACKNOWLEDGED
          |
          v
Fleet Telemetry later reports 12A
          |
          v
Command EFFECT_CONFIRMED
```

If no trustworthy observation verifies the effect before the verification deadline, the command remains unconfirmed or moves to `UNKNOWN_OUTCOME`/`FAILED` according to command policy.

## 12.3 Command semantics classes

Every command type declares retry semantics.

```text
IDEMPOTENT_SET
EDGE_TRIGGERED
NON_REPEATABLE
```

Examples:

- `SetChargeLimit(80)` -> generally modeled as `IDEMPOTENT_SET`.
- `ReserveResource` -> idempotent only with the same reservation/idempotency key.
- A future provider action that could create a duplicate paid transaction -> `NON_REPEATABLE` unless provider idempotency is guaranteed.

Adapters may not blindly retry unknown operations.

---

# 13. Command policy gate

Before a provider/resource command leaves CabOps, validate:

- tenant/assignment authorization
- target exists and is active
- capability is supported
- provider authorization is valid
- command is not expired
- required state is fresh enough
- stated preconditions still hold
- conflicting hold/restriction does not prohibit action
- safety class permits automated execution
- idempotency/retry policy permits dispatch

Rejections are durable facts with reason codes.

Example reason codes:

```text
CAPABILITY_UNKNOWN
CAPABILITY_UNSUPPORTED
STALE_STATE
VEHICLE_OUT_OF_SERVICE
INCIDENT_HOLD
AUTHORIZATION_MISSING
RESOURCE_CONFLICT
COMMAND_EXPIRED
PRECONDITION_FAILED
POLICY_DENIED
```

---

# 14. Optimistic concurrency and preconditions

Commands that depend on mutable state should be able to include expected versions or predicates.

Examples:

```text
expected_vehicle_state_version = 18422
expected_charger_assignment = C-07
expected_soc_range = [20, 25]
expected_maintenance_state = SERVICEABLE
```

If critical preconditions no longer hold, re-plan rather than execute stale intent.

This is especially important for CabEnergy, CabRoute, CabDepot resource assignment, and future autonomous dispatch.

---

# 15. Event delivery, idempotency, and ordering

## 15.1 Delivery model

Assume:

```text
AT_LEAST_ONCE
```

Consumers persist processed event/message IDs or an equivalent idempotency mechanism.

## 15.2 Outbox / inbox pattern

Production persistence should support a transactional outbox for internally generated domain events and an inbox/deduplication mechanism for consumers.

Logical flow:

```text
Domain transaction
   +--> state/record mutation
   +--> outbox event
       committed together

Publisher
   +--> event bus

Consumer
   +--> inbox/dedup
   +--> apply idempotently
```

No implementation may rely on a database commit and message publish magically succeeding atomically without such a pattern or equivalent guarantee.

## 15.3 External observation deduplication

Use provider event IDs when trustworthy.

When no provider event ID exists, deduplication may use a deterministic fingerprint over normalized fields such as:

```text
provider
provider_vehicle_id
observation_type/field
provider_timestamp/observed_at
normalized value
```

High-frequency telemetry deduplication must not destroy materially distinct samples.

## 15.4 Out-of-order observations

A late valid observation is:

1. persisted to history,
2. evaluated against current field version/event time,
3. ignored for current-state regression if older than the authoritative current observation,
4. still available for analytics/replay.

Current projections must never simply apply `last message received wins` across all sources.

---

# 16. Capability negotiation contract

Each provider/resource exposes a capability profile.

```text
CapabilityRecord

subject_type
subject_id
capability
status
constraints
source
last_verified_at
expires_at?
version
```

Status:

```text
SUPPORTED
UNSUPPORTED
UNKNOWN
DEGRADED
TEMPORARILY_UNAVAILABLE
```

Examples:

```text
TELEMETRY_STREAMING
SET_CHARGE_CURRENT
CABIN_CAMERA_FRAME
AUTONOMOUS_REPOSITION
ROBOTAXI_NETWORK_CONTROL
WIRELESS_CHARGING
```

### Hard rule

Business modules ask for capabilities, not vehicle models.

Bad:

```text
if vehicle.model == CYBERCAB: reposition()
```

Good:

```text
if capability(AUTONOMOUS_REPOSITION) == SUPPORTED:
    request_reposition()
```

This is the mechanism that isolates P0.10A's Cybercab capability gates.

---

# 17. Depot visit model

A depot visit is a durable operational episode.

```text
DepotVisit

visit_id
vehicle_id
management_assignment_id
hub_id
expected_arrival_at?
arrived_at?
departed_at?
visit_reason
priority
status
```

Recommended lifecycle:

```text
EXPECTED
ARRIVING
ONSITE
READY_FOR_DEPARTURE
DEPARTED
CANCELLED
```

Do **not** model the entire internal facility process as one serial visit state machine, because cleaning, charging, inspection, and maintenance can overlap or occur in different orders.

Instead, a visit owns/links multiple jobs and resource reservations.

```text
DepotVisit
   +--> Inspection job
   +--> Cleaning work order
   +--> Maintenance work order
   +--> Charging plan/assignment
   +--> Incident case
   +--> Resource reservations
```

This prevents state explosion.

---

# 18. Resource reservation contract

CabDepot manages scarce physical resources.

```text
ResourceReservation

reservation_id
hub_id
resource_id
vehicle_id?
visit_id?
work_order_id?

start_at
end_at?
priority
status
purpose
```

Status:

```text
HELD
CONFIRMED
OCCUPIED
RELEASED
EXPIRED
CANCELLED
CONFLICT
```

Examples of resources:

```text
wash bay
interior bay
exception bay
maintenance bay
charger
parking position
inspection portal
```

Physical occupancy from sensors/operators can disagree with planned reservation state. CabDepot preserves both and raises a conflict rather than overwriting evidence.

---

# 19. Work-order contract

CabCare and CabMaint share a generic work-order shape but own different work types and policies.

```text
WorkOrder

work_order_id
work_order_domain       # CARE / MAINT
vehicle_id
visit_id?
incident_id?

work_type
priority
status
created_at
required_by?
assigned_resource_id?
assigned_operator_id?

estimated_duration?
actual_started_at?
actual_completed_at?
completion_evidence?
```

Lifecycle:

```text
OPEN
PLANNED
READY
IN_PROGRESS
BLOCKED
COMPLETED
CANCELLED
FAILED
```

A completed work order emits a domain event; it does not directly mutate another bounded context's authoritative state without that context processing the event.

---

# 20. CabVision finding contract

CabVision emits findings rather than issuing operational actions directly.

```text
InspectionFinding

finding_id
vehicle_id
visit_id?
inspection_id

finding_type
severity
confidence
location_on_vehicle?
observed_at
source
model_version?

evidence_reference?
retention_class
status
```

Finding status:

```text
DETECTED
CONFIRMED
DISMISSED
RESOLVED
SUPERSEDED
```

Examples:

```text
TRASH
LIQUID_SPILL
SUSPECTED_BIOLOGICAL_CONTAMINATION
LOST_PROPERTY
UPHOLSTERY_DAMAGE
GLASS_DAMAGE
BODY_DAMAGE
TIRE_VISUAL_ANOMALY
```

CabIncident determines whether a finding becomes an incident and coordinates consequences.

---

# 21. Incident contract

```text
Incident

incident_id
vehicle_id
management_assignment_id
visit_id?

incident_type
severity
status
opened_at
resolved_at?

source_finding_ids[]
service_hold
owner_notification_required
insurance_relevant
billing_relevant
retention_class
```

Status:

```text
OPEN
TRIAGED
MITIGATING
AWAITING_EXTERNAL
RESOLVED
CLOSED
CANCELLED
```

Incident types may include:

```text
SPILL
BIOLOGICAL_CONTAMINATION
LOST_PROPERTY
INTERIOR_DAMAGE
EXTERIOR_DAMAGE
VANDALISM
COLLISION
SAFETY_EVENT
THEFT_ATTEMPT
SERVICE_INTERRUPTION
OTHER
```

Incident severity and service hold are separate fields; a low-severity lost item may not hold the vehicle, while a contamination event may.

---

# 22. Maintenance restriction contract

CabMaint may place explicit operational restrictions.

```text
MaintenanceRestriction

restriction_id
vehicle_id
reason
severity
allowed_operations
prohibited_operations
applied_at
expires_at?
cleared_at?
source
```

Examples:

```text
NO_REVENUE_SERVICE
NO_HIGHWAY_OPERATION
DEPOT_ONLY
CHARGE_ALLOWED
TOW_REQUIRED
```

CabDispatch and command policy must honor active restrictions.

---

# 23. Readiness derivation

Overall readiness is computed from multiple bounded contexts.

Conceptually:

```text
READY if
    lifecycle == ACTIVE
    AND no blocking incident hold
    AND maintenance permits planned operation
    AND care state satisfies requirement
    AND energy satisfies active readiness requirement
    AND required provider capability/access is available
    AND required depot/route condition is satisfied
```

Return structured reasons, not only a boolean.

Example:

```text
VehicleReadiness

status = NOT_READY
reasons = [
  ENERGY_TARGET_NOT_MET,
  DEEP_CLEAN_REQUIRED
]
next_required_ready_at = 05:30
estimated_ready_at = 05:12
confidence = 0.88
```

This enables explainable owner/operator dashboards and planning.

---

# 24. Failure and retry semantics

Failures are classified, not flattened into one error string.

```text
TRANSIENT
RATE_LIMITED
AUTHORIZATION
CAPABILITY
PRECONDITION
CONFLICT
PROVIDER_REJECTED
TIMEOUT
UNKNOWN_OUTCOME
PERMANENT
POLICY
DATA_QUALITY
```

Retry policy is determined by:

- failure class,
- command semantics class,
- provider idempotency support,
- command expiration,
- current preconditions,
- safety class.

### Example

A timeout after sending an edge-triggered provider command is **not** automatically safe to retry. First reconcile state if possible.

Unknown outcome must remain an explicit state.

---

# 25. Dead-letter / quarantine behavior

Messages that cannot be safely processed after the configured retry budget go to a durable quarantine/dead-letter path with:

```text
message_id
message_type
failure_class
last_error
attempt_count
first_failed_at
last_failed_at
payload_reference
correlation_id
operator_action_required
```

Do not silently drop malformed provider telemetry or failed operational events.

Schema incompatibility and unknown enum values must be visible operational faults.

---

# 26. Audit contract

Domain events are not a complete security/audit log.

Use a separate append-only audit record for material access/actions.

```text
AuditRecord

audit_id
occurred_at
actor_type
actor_id?
tenant_id?

operation
object_type
object_id?
result
reason_code?

request_id?
correlation_id?
source_ip_or_device_context?   # where lawful/relevant
metadata?
```

Audit should cover at least:

- successful and denied privileged commands
- management/ownership assignment changes
- capability/authorization changes
- operator overrides
- incident evidence access
- financial adjustments
- policy overrides
- manual state corrections

Audit records are append-only. Corrections create new records; they do not rewrite prior audit history.

---

# 27. Human override contract

Automation must support explicit operator override without destroying provenance.

An override records:

```text
override_id
actor_id
scope
previous_value/action
new_value/action
reason_code
freeform_reason?
created_at
expires_at?
```

The system must distinguish:

- automated decision,
- provider-reported state,
- operator-confirmed observation,
- operator override.

No hidden manual database edits are considered an acceptable production workflow.

---

# 28. Schema/versioning policy

Every durable event, observation, command, plan, and API contract has a `schema_version`.

Rules:

1. Historical records are immutable.
2. Additive backward-compatible fields may evolve within the supported schema lineage.
3. Breaking semantic changes require a new schema major/version or event type.
4. Consumers must tolerate unknown additive fields.
5. Unknown enum values must not crash ingestion; map to `UNKNOWN`/quarantine according to safety relevance while preserving the raw value.
6. Replayers use the schema version that existed when the record was written or an explicitly versioned upcaster.
7. Provider adapter versions are recorded on normalized observations where useful for forensic replay.

---

# 29. Projection rebuilding and replay

Critical projections must be rebuildable from durable canonical history plus authoritative snapshots where high-volume telemetry retention requires compaction.

Do not make a current-state cache the only copy of business truth.

Replay must support:

- deterministic ordering of internal aggregate events,
- event-time handling for external observations,
- schema upcasting,
- idempotent consumers,
- simulator reproduction,
- historical model/decision analysis.

For very high-volume telemetry, a retention/compaction policy may preserve canonical time-series history without retaining every raw provider payload forever. Exact retention is a later data-lifecycle decision.

CabVision raw imagery follows the separate privacy/evidence policy from P0.10A/P0.11 and is not treated like ordinary telemetry.

---

# 30. API boundary principles

External/operator APIs should primarily expose:

- current-state/read projections,
- explicit commands/intents,
- work/incident actions,
- plan/readiness management,
- history/audit where authorized.

Do not expose provider-native payloads as the main public contract.

Conceptual API surface:

```text
GET  /vehicles/{id}
GET  /vehicles/{id}/readiness
GET  /vehicles/{id}/history
GET  /hubs/{id}/state
GET  /incidents
GET  /work-orders

POST /readiness-requirements
POST /commands
POST /incidents/{id}/actions
POST /work-orders/{id}/actions
POST /operator-overrides
```

Exact REST/GraphQL/RPC choices are intentionally not frozen in P0.10B.

---

# 31. NexusOS integration contract

NexusOS operates at higher intent/policy level.

Good NexusOS interactions:

```text
Set Hub 001 site-demand ceiling for this policy window.
Identify vehicles at risk of missing morning readiness.
Apply an approved fleet capital/operations policy.
Request a fleet readiness summary.
Request a re-optimization after a site outage.
```

NexusOS should not send provider-native commands such as Tesla-specific API method names.

Flow:

```text
NexusOS goal/policy
      |
      v
CabOps command/requirement/plan API
      |
      v
CabOps bounded context
      |
      v
Provider/resource adapter
```

---

# 32. Simulator parity contract

The simulator is a provider/resource implementation, not a separate architecture.

```text
ProviderAdapter
    TeslaAdapter
    FutureAVAdapter
    SimulatorVehicleAdapter

SiteEnergyProvider
    RealMeter/EVSE providers
    SimulatorSiteEnergyProvider
```

Requirements:

- simulated vehicles emit the same canonical observations,
- simulated provider commands use the same command ledger,
- simulated failures use the same failure classes,
- simulated hubs use the same CabDepot resources/reservations,
- CabEnergy/CabRoute/CabMaint/CabCare/CabDispatch do not know whether the target is real or simulated,
- simulation supports deterministic random seeds,
- simulation supports accelerated/virtual time while preserving domain timestamps consistently.

This is essential for mixed-fleet tests such as `1 real Tesla + 99 simulated vehicles`.

---

# 33. Logical persistence inventory

P0.10B does not choose a database engine, but production persistence must support logical records equivalent to:

```text
organizations
users / service identities
tenants
vehicles
vehicle_management_assignments
provider_accounts
provider_vehicle_links
capability_records

canonical_observations
domain_events
outbox
consumer_inbox
command_ledger
audit_records

readiness_requirements
operational_plans

depot_visits
resources
resource_reservations
work_orders
inspection_findings
incidents
maintenance_restrictions

financial facts / settlements
current-state projections
```

High-volume telemetry/time-series storage may use a specialized physical store later, but its canonical identity/time/tenancy semantics remain the same.

---

# 34. Core invariants

These invariants are freeze-candidate requirements.

## Identity

- Internal `vehicle_id` never changes because a provider ID/account changes.
- Tenant/owner/manager relationships are effective-dated and history preserving.
- Provider IDs never become cross-domain primary keys.

## Events/history

- Durable domain events are immutable.
- Internal aggregate sequence never moves backward.
- Current projections are rebuildable or reconcilable from durable authority.

## Commands

- Every external side-effecting command has a durable `command_id`.
- Provider acknowledgement is distinct from effect confirmation.
- Unknown outcome is representable.
- Retry semantics are explicit per command type.
- Safety-relevant unknown capability fails closed.

## Tenancy/security

- Every customer-scoped operational record carries sufficient tenancy/assignment context.
- Cross-tenant access requires explicit platform/operator authorization.
- Reassignment of a vehicle does not silently transfer historical tenant visibility.

## Readiness

- Overall readiness is derived with reason codes.
- CabEnergy does not invent dispatch/business deadlines.
- Maintenance/incident holds override revenue availability when policy says they are blocking.

## Provider neutrality

- Core modules do not depend on Tesla field names/API calls.
- Simulator and real adapters obey the same contracts.

---

# 35. Initial event taxonomy

The following naming families are approved as a starting registry. Exact payload schemas are defined in later implementation contracts.

## Fleet Registry

```text
VehicleRegistered
VehicleActivated
VehicleSuspended
VehicleRetired
ManagementAssignmentStarted
ManagementAssignmentEnded
ProviderLinkEstablished
ProviderLinkLost
CapabilityProfileChanged
```

## CabDispatch

```text
ReadinessRequirementCreated
ReadinessRequirementSuperseded
ReadinessRequirementSatisfied
ReadinessRequirementMissed
VehicleAvailabilityChanged
ProviderAssignmentObserved
ServiceHoldApplied
ServiceHoldCleared
```

## CabDepot

```text
DepotVisitExpected
VehicleArrivedAtHub
VehicleDepartedHub
ResourceReserved
ResourceOccupied
ResourceReleased
ResourceConflictDetected
```

## CabVision / CabCare / CabIncident

```text
InspectionCompleted
InspectionFindingDetected
InspectionFindingConfirmed
IncidentOpened
IncidentSeverityChanged
IncidentResolved
CareWorkOrderCreated
CareWorkStarted
CareWorkCompleted
```

## CabMaint

```text
MaintenanceAlertObserved
MaintenanceRestrictionApplied
MaintenanceRestrictionCleared
MaintenanceWorkOrderCreated
MaintenanceWorkCompleted
```

## CabEnergy

```text
EnergyReadinessChanged
ChargingPlanCreated
ChargingPlanActivated
ChargingPlanSuperseded
ChargingAssignmentChanged
SitePowerConstraintChanged
```

## CabRoute

```text
RoutePlanCreated
PreferredSiteChanged
RepositionIntentCreated
RepositionIntentCancelled
```

## Commands

```text
CommandRequested
CommandRejected
CommandDispatched
CommandAcknowledged
CommandEffectConfirmed
CommandFailed
CommandExpired
CommandUnknownOutcome
```

## CabRevenue

```text
RevenueFactRecorded
CostFactRecorded
SettlementImported
OwnerStatementGenerated
FinancialAdjustmentRecorded
```

Event names describe completed facts, not instructions.

---

# 36. What P0.10B intentionally does not freeze

The following remain implementation or later-domain decisions:

- programming language/framework
- database engine
- event-bus product
- Kafka vs another broker
- REST vs GraphQL vs RPC
- exact table DDL/indexes
- exact telemetry retention duration
- exact CV image retention duration
- exact SLAs/timeouts/retry counts
- exact charger scheduling algorithm
- exact routing optimizer
- exact user/role catalog
- final revenue accounting ledger model
- final facility sensor vendors
- Cybercab-specific capabilities that remain gated in P0.10A

These choices must conform to the contracts above rather than redefine them.

---

# 37. Implementation sequence after contract freeze

Recommended order:

1. Typed ID/value-object library.
2. Tenant/organization/vehicle/assignment registry.
3. Canonical observation envelope and ingestion interface.
4. Domain-event envelope and event registry.
5. Transactional outbox + consumer inbox/idempotency.
6. Current-state projector framework.
7. Capability registry/resolver.
8. Durable command ledger + policy gate + verifier.
9. Simulator provider and virtual clock.
10. CabDispatch readiness requirement/readiness projection.
11. CabDepot visit/resource contracts.
12. CabEnergy deterministic V0 contracts.
13. CabMaint/CabCare work-order infrastructure.
14. CabVision finding + CabIncident case infrastructure.
15. CabRoute plan/intents.
16. CabRevenue economic facts.
17. NexusOS API boundary.
18. TeslaAdapter plugs into the same observation/command interfaces from P0.10A.

This sequence creates the durable skeleton before optimization/ML or UI complexity.

---

# 38. Freeze decision

## P0.10B core domain/event contract

**FREEZE CANDIDATE**

Freeze-candidate decisions:

- provider-neutral bounded contexts
- CabCare and CabIncident as first-class domains
- tenant/owner/manager/payer separation
- effective-dated vehicle management assignments
- observation vs domain event vs command vs projection vs audit separation
- multi-dimensional vehicle state rather than one giant status enum
- readiness requirements owned by CabDispatch
- durable operational plans and reason-coded decisions
- command request/acknowledgement/effect-verification lifecycle
- at-least-once delivery with idempotent consumers
- transactional outbox/inbox pattern or equivalent guarantee
- event-time-aware out-of-order observation handling
- explicit capability negotiation
- visit + parallel work/resource model instead of a serial depot mega-state-machine
- explicit unknown-outcome failure state
- append-only audit record
- same contracts for simulator and real providers
- NexusOS remains above CabOps and provider-neutral

## Open implementation proof gates

Before final `FROZEN` status, implementation/prototype work should prove:

1. out-of-order telemetry cannot regress projected current state;
2. duplicate events/commands are harmless under retry;
3. provider acknowledgement and physical effect can be reconciled independently;
4. a vehicle can change owner/manager without leaking historical tenant data;
5. one real provider adapter and the simulator can drive identical core workflows;
6. a depot visit supports simultaneous charging + cleaning without state-machine conflict;
7. a blocking incident/maintenance restriction reliably removes dispatch readiness;
8. replay rebuilds current-state projections deterministically from the chosen durable history model.

---

# 39. Next architecture pass

Recommended next pass:

**P0.10C — CabEnergy Deterministic Constraint & Scheduling Contract**

Reason:

CabEnergy is already a Phase 0 freeze-candidate subsystem and is the most operationally critical software function for a Sacramento depot. With P0.10B's shared contracts in place, P0.10C can define the real optimization problem before ML:

- vehicle readiness constraints
- charger/connector constraints
- site kW limit
- tariff/time-of-use cost
- demand-charge state
- charge-rate constraints
- assignment/scheduling variables
- priority and missed-readiness penalties
- plan/replan triggers
- degradation term interface
- deterministic baseline solver
- simulator test scenarios
- future forecast/ML plug-in boundaries

The production rule should remain: **define the deterministic feasible operating problem first; add forecasting/ML only where it improves a measured decision.**

---

# Change Log

## 2026-09-04 — P0.10B documented

Created the provider-neutral CabOps Core Domain & Event Contracts.

Major additions:

- formal bounded-context responsibilities;
- CabCare/CabIncident first-class software domains;
- history-preserving tenant/owner/manager identity model;
- canonical observation/event/command/plan/audit separation;
- multi-dimensional vehicle state and derived readiness;
- operational-plan and reason-code model;
- durable command verification lifecycle;
- at-least-once/idempotency/outbox/inbox semantics;
- event-time ordering rules;
- capability negotiation;
- depot visit/resource/work-order contracts;
- inspection finding / incident / maintenance restriction contracts;
- failure/dead-letter/operator-override rules;
- simulator parity requirements;
- core invariants and initial event taxonomy.

P0.10B is now a **freeze candidate**, pending implementation proof gates listed above.
