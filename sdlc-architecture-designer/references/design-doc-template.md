# Design Document Template

Use this template when generating a technical design document. Scale section depth to the feature complexity and chosen methodology.

---

# Design Doc: [Feature Name]

**Author:** [Name]
**Date:** [Date]
**Status:** Draft | In Review | Approved
**PRD Reference:** [Link or title]

---

## 1. Overview

Brief summary of the design: what is being built, the key architectural decisions, and how it fits into the existing system.

## 2. Goals and Non-Goals

### Goals
- [What this design achieves]

### Non-Goals
- [What this design explicitly does not address]

## 3. Architecture

### System Context

Present the system context diagram showing how this feature fits into the broader system.

```text
[ASCII system context diagram here]
```

### Component / Container View

Show the internal architecture — services, modules, and their relationships.

```text
[ASCII component diagram here]
```

### Clean Architecture Layers

```text
+-----------------------------+
| Interface Adapters          |
| Controller / API / UI       |
+-----------------------------+
              |
              v
+-----------------------------+
| Application Layer           |
| Use Cases / Orchestration   |
+-----------------------------+
              |
              v
+-----------------------------+
| Domain Layer                |
| Entities / Rules / Policies |
+-----------------------------+
              |
              v
+-----------------------------+
| Infrastructure              |
| DB / Queue / External APIs  |
+-----------------------------+
```

Describe what lives in each layer for this feature.

## 4. Domain Model

### Bounded Contexts

Identify DDD bounded contexts and their relationships.

```text
[ASCII bounded context map here]
```

### Aggregates and Entities

| Aggregate | Entities | Value Objects | Key Invariants |
|-----------|----------|---------------|----------------|
| | | | |

### Domain Events

| Event | Trigger | Consumers | Payload Summary |
|-------|---------|-----------|-----------------|
| | | | |

## 5. API and Interface Contracts

### Endpoint / Interface: [Name]

**Design by Contract specification:**

```text
Contract: [OperationName](params)

Preconditions:
- [condition 1]
- [condition 2]

Postconditions:
- [outcome 1]
- [outcome 2]

Invariants:
- [invariant 1]

Error Conditions:
- [condition] -> [error response]
```

**Request:**
```
[Method] [Path]
{request body}
```

**Response:**
```
[Status code]
{response body}
```

(Repeat for each endpoint / interface)

## 6. Data Flow

### Request Flow (Sequence Diagram)

```text
[ASCII sequence diagram here]
```

### State Transitions (if applicable)

```text
[ASCII state diagram here]
```

## 7. Data Model

### Schema Changes

| Table / Collection | Change | Fields | Notes |
|--------------------|--------|--------|-------|
| | | | |

### Data Migration

- Migration strategy
- Backward compatibility approach
- Rollback plan for data changes

## 8. Infrastructure and 12-Factor Considerations

| Factor | How Addressed |
|--------|---------------|
| Config | |
| Dependencies | |
| Backing Services | |
| Stateless Processes | |
| Port Binding | |
| Concurrency | |
| Disposability | |
| Dev/Prod Parity | |
| Logs | |
| Admin Processes | |

(Include only factors relevant to this feature)

## 9. Security Considerations (ISO 27001 Awareness)

- Data classification of information handled
- Access control and authorization approach
- Audit logging and traceability
- Encryption (at rest, in transit)
- Least privilege considerations
- Compliance implications

## 10. Observability

- Key metrics to track
- Logging strategy
- Alerting thresholds
- Dashboard requirements
- Distributed tracing approach

## 11. Failure Handling

- Failure scenarios and recovery strategies
- Circuit breaker / retry patterns
- Graceful degradation approach
- Data consistency guarantees

## 12. Trade-offs and Alternatives

### Chosen Approach
- Why this approach was selected

### Alternatives Considered
| Alternative | Pros | Cons | Why Rejected |
|------------|------|------|-------------|
| | | | |

## 13. Implementation Plan

- Milestone sequencing
- Dependency order
- Estimated work breakdown
- Rollout strategy (feature flag, canary, phased, etc.)

## 14. Open Questions

- [ ] [Question 1]
- [ ] [Question 2]
