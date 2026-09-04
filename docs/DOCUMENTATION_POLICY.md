# CyberCab Documentation Policy

**Effective:** 2026-09-04  
**Applies to:** `OneVillage83/CyberCab`

CyberCab is a documentation-first project. The repository must preserve enough context that architecture, software, facility, financial, regulatory, vendor, and operating decisions do not have to be rediscovered later.

## 1. Required documentation behavior

For every material change, record:

1. **What changed.**
2. **Why it changed.**
3. **What evidence or assumption motivated it.**
4. **What alternatives were considered or rejected, when material.**
5. **What external dependencies or unresolved gates remain.**
6. **What files, contracts, models, or systems are affected.**
7. **What the next expected step is.**

A material change includes, but is not limited to:

- architecture decisions
- domain/event/API contract changes
- Tesla or other provider integration changes
- source/data-provider changes
- software implementation changes
- configuration or environment changes
- security/permission changes
- physical facility design decisions
- wash/charging/inspection equipment decisions
- utility/power assumptions
- pricing and customer-contract changes
- property/location assumptions
- insurance/regulatory assumptions
- fleet economics changes
- ML/optimization model changes
- deployment/infrastructure changes

## 2. Timestamped project log

Every material change must also receive a dated note in [`PROJECT_LOG.md`](PROJECT_LOG.md).

Use this format:

```markdown
## YYYY-MM-DD — Short title

**Area:** architecture / software / business / facility / provider / regulation / finance / operations

**Changed:**
- ...

**Reason:**
- ...

**Open gates / follow-up:**
- ...

**Affected docs/code:**
- `path/to/file`
```

When exact time matters, include the time and timezone.

## 3. Architecture freeze discipline

Important architecture documents should explicitly state one of:

- `DRAFT`
- `ACTIVE DESIGN`
- `FREEZE CANDIDATE`
- `FROZEN`
- `SUPERSEDED`

A `FREEZE CANDIDATE` is stable enough for dependent work to begin but may still contain clearly listed external/open gates.

A `FROZEN` contract may only be changed deliberately. If it changes, the change log must explain why and identify downstream contracts that need review.

## 4. Assumptions must be visible

Never encode an uncertain Tesla, Cybercab, regulatory, utility, or market capability as if it were fact.

Use explicit statuses such as:

- `AVAILABLE`
- `PARTIAL`
- `BUILD`
- `CYBERCAB-GATED`
- `UNVERIFIED`
- `NOT PUBLICLY EXPOSED`
- `BLOCKED`

The purpose is to keep unknowns as visible external gates rather than hidden architectural debt.

## 5. Provider-neutrality rule

CabOps domain contracts must remain provider-neutral unless a document explicitly concerns a provider adapter.

Tesla-specific behavior belongs behind `TeslaAdapter` or a Tesla-specific document. The same rule applies to future AV, charging, energy, maintenance, mapping, or payment providers.

## 6. Evidence and references

When a decision depends on an external source, preserve enough information to re-check it later:

- provider/agency/vendor name
- page/document title
- source URL when appropriate
- date checked
- exact capability or constraint relied upon

Do not rely on an Instagram/TikTok/marketing claim as production evidence without validating the actual vendor specification, utility rule, regulation, or API documentation.

## 7. Physical and software systems are one architecture

CyberCab is both a physical operations business and a software platform. Documentation should therefore connect:

- vehicle state
- depot state
- charging state
- cleaning/inspection state
- maintenance state
- customer/owner state
- revenue/cost state
- site power state
- commands and audit history

A facility change that affects software state or vice versa must be documented across the relevant boundary.

## 8. Documentation-before-memory rule

Conversation history or human memory is not the authoritative project record.

When a decision is important enough that we would be annoyed to rediscover it later, it belongs in the repository.

## 9. Initial repository structure

```text
docs/
  DOCUMENTATION_POLICY.md
  PROJECT_LOG.md
  phase-0/
    PHASE_0_OVERVIEW.md
    PHASE_0_STATUS.md
    P0_10A_TESLA_INTEGRATION_CONTRACT.md
```

Additional folders should be added as the project matures, for example:

```text
docs/
  architecture/
  business/
  facilities/
  integrations/
  operations/
  security/
  economics/
  decisions/
```

## 10. Current next step

The next architecture document is:

**P0.10B — CabOps Core Domain & Event Contracts**

It should define the stable production vocabulary shared by CabEnergy, CabMaint, CabRoute, CabVision, CabDepot, CabRevenue, provider adapters, and NexusOS.
