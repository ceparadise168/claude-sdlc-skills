# Test Plan Template

Use this template when generating a test plan. Scale to the feature size — small features may need only a subset of sections.

---

# Test Plan: [Feature Name]

**Author:** [Name]
**Date:** [Date]
**Status:** Draft | In Review | Approved
**PRD Reference:** [Link or title]
**Design Doc Reference:** [Link or title]

---

## 1. Overview

Brief summary of what is being tested and the overall test strategy.

## 2. Test Scope

### In Scope
- [Feature/component/flow to test]

### Out of Scope
- [What is explicitly not tested in this plan]

## 3. Test Strategy by Layer

### Unit Tests
**Purpose:** Verify domain logic, business rules, and individual component behavior.
**Owner:** Development team
**Framework:** [Project's unit test framework]

| Area | What to Test | Key Scenarios |
|------|-------------|---------------|
| Domain logic | | |
| Business rules | | |
| Utility functions | | |

### Integration Tests
**Purpose:** Verify service boundaries, API contracts, and component interactions.
**Owner:** Development team
**Framework:** [Project's integration test framework]

| Integration Point | What to Test | Key Scenarios |
|-------------------|-------------|---------------|
| API endpoints | | |
| Database operations | | |
| External service calls | | |

### Contract Tests
**Purpose:** Verify Design by Contract specifications — preconditions, postconditions, invariants.
**Owner:** Development team

| Contract | Preconditions to Verify | Postconditions to Verify | Invariants |
|----------|------------------------|-------------------------|------------|
| | | | |

### E2E Tests (Playwright — required when UI exists)
**Purpose:** Validate critical user flows end-to-end through the UI.
**Owner:** QA / Development team
**Framework:** Playwright

| User Flow | Steps | Expected Outcome | Priority |
|-----------|-------|-------------------|----------|
| Happy path: [flow] | | | P0 |
| Error path: [flow] | | | P0 |
| Edge case: [flow] | | | P1 |

### Edge Case and Resilience Tests
**Purpose:** Verify the system behaves correctly under abnormal, adversarial, and boundary conditions. These tests catch bugs that happy-path testing misses — the ones that cause production incidents.
**Owner:** Development team + QA

#### Concurrency & Race Conditions

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| Concurrent writes to same resource | Two requests update the same record simultaneously | One succeeds, other gets 409 Conflict or retries via optimistic lock |
| Database lock contention | Long transaction blocks another | Blocked transaction waits or times out gracefully, no deadlock |
| Deadlock detection | Two transactions lock resources in opposite order | Database detects deadlock, one transaction rolled back, caller retries |
| Connection pool exhaustion | All DB connections in use | New requests get queued or return 503, no crash |

#### Boundary Values & Invalid Input

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| Large numbers | Max int, max float, overflow values | Proper validation or graceful handling, no crash |
| Zero / negative values | amount=0, quantity=-1, negative IDs | 400 Bad Request with clear error message |
| Empty / null values | Empty strings, null fields, missing required fields | 400 with field-level validation errors |
| Max-length strings | String at max length, string exceeding max | Accepted at max, rejected above with clear error |
| Malformed payloads | Invalid JSON, wrong content-type, truncated body | 400 Bad Request, no server error |
| Unexpected fields | Extra fields in request body | Ignored or rejected per API contract |
| Unicode / special characters | Emoji, RTL text, null bytes, control characters | Handled correctly, no encoding errors |

#### Security & Injection

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| SQL injection | `'; DROP TABLE users; --` in text fields | Input sanitized, query parameterized, no execution |
| XSS | `<script>alert('xss')</script>` in input fields | Output escaped, script not executed |
| Command injection | `; rm -rf /` in system-facing inputs | Input rejected or sanitized |
| Path traversal | `../../etc/passwd` in file paths | Resolved to safe path, access denied |
| Header injection | Newlines in header values | Rejected or sanitized |

#### Authentication & Authorization (HTTP Error Codes)

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| 401 Unauthenticated | No token, expired token, malformed token | 401 response, no data leaked |
| 403 Forbidden | Valid user, insufficient permissions | 403 response, action not performed |
| Token expiry during request | Token expires mid-session | Graceful re-auth prompt or 401 |
| Role escalation attempt | User modifies role claim in token | Rejected, audit logged |
| CSRF | Cross-site request without CSRF token | Rejected |

#### Client Error Responses

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| 400 Bad Request | Validation failures, type mismatches | Clear error with field-level details |
| 404 Not Found | Request for non-existent resource | 404, no information leakage |
| 409 Conflict | Duplicate creation, stale update | 409 with conflict details |
| 422 Unprocessable | Syntactically valid but semantically wrong | 422 with business rule explanation |
| 429 Too Many Requests | Exceeding rate limit | 429 with Retry-After header |

#### Network & Infrastructure Failures

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| Bad connection / timeout | External service unreachable | Circuit breaker opens, fallback or 503 |
| DNS failure | DNS resolution fails | Timeout with clear error, no hang |
| Slow response | External service responds after 30s | Request timeout triggers, no thread leak |
| Connection pool exhaustion | All outbound connections in use | Backpressure or 503, no crash |
| Partial response | External service sends incomplete data | Detected and handled, no corrupt state |

#### Database Edge Cases

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| DB lock timeout | Transaction can't acquire lock within limit | Timeout error, transaction rolled back cleanly |
| Transaction rollback | Failure mid-transaction | All changes rolled back, no partial state |
| Migration failure | Schema migration fails halfway | Rollback to previous schema, no data loss |
| Stale read | Read after write in eventually consistent system | Documented behavior, no silent data loss |

#### Data Consistency & Idempotency

| Scenario | What to Test | Expected Behavior |
|----------|-------------|-------------------|
| Duplicate request | Same request sent twice (network retry) | Idempotent result, no duplicate side effects |
| Partial write | Crash between two related writes | Atomic or compensating, no orphaned data |
| Eventual consistency gap | Read immediately after write in distributed system | Documented, handled in UI/UX if applicable |

### Stress Tests (mandatory before launch)
**Purpose:** Verify the system behaves correctly under production-level and beyond-production-level load. Stress testing is not optional — it's how you discover bottlenecks, memory leaks, connection pool limits, and degradation patterns before your users do.
**Owner:** Development team + SRE/Ops
**Framework:** [k6 / Artillery / Locust / Gatling / JMeter — pick one]

See `references/stress-test-patterns.md` for detailed patterns.

#### Load Tests (sustained traffic)
Simulate expected production traffic over a sustained period.

| Scenario | Virtual Users | Duration | Target Metric | Acceptance Threshold |
|----------|--------------|----------|--------------|---------------------|
| Normal load | [expected concurrent users] | 15-30 min | p95 latency | < [target] ms |
| Peak load | [2x expected] | 15 min | p99 latency | < [target] ms |
| Sustained load | [expected] | 2-4 hours | Error rate | < 0.1% |

#### Spike Tests (sudden traffic bursts)
Simulate sudden surges — flash sales, viral events, batch job kicks.

| Scenario | Ramp Pattern | Target Metric | Acceptance Threshold |
|----------|-------------|--------------|---------------------|
| Traffic spike | 0 → [10x normal] in 30s | Recovery time | < [target] seconds |
| Traffic drop | [10x] → [normal] in 10s | No errors during scale-down | 0 errors |

#### Soak Tests (endurance)
Run at moderate load for extended periods to catch memory leaks, connection pool exhaustion, and gradual degradation.

| Scenario | Duration | What to Monitor |
|----------|----------|-----------------|
| Extended run | 4-12 hours | Memory usage trend (should be flat, not climbing) |
| | | DB connection pool usage |
| | | Thread/goroutine count |
| | | Response time degradation over time |
| | | Disk usage (logs, temp files) |

#### Capacity Tests (find the breaking point)
Incrementally increase load until the system degrades or breaks. This tells you your actual capacity ceiling.

| Step | Virtual Users | Measure |
|------|--------------|---------|
| Baseline | [normal] | p50, p95, p99 latency, error rate, CPU, memory |
| +50% | [1.5x] | Same metrics |
| +100% | [2x] | Same metrics |
| +200% | [3x] | Same metrics — where does it break? |

#### Stress Test Exit Criteria
- [ ] System handles expected peak load with p95 < target latency
- [ ] Error rate stays below 0.1% under normal and peak load
- [ ] No memory leaks detected during 4+ hour soak test
- [ ] Graceful degradation under overload (503 with backpressure, not crash)
- [ ] Recovery time after spike < [target] seconds
- [ ] Breaking point documented and is above expected peak with safety margin

### Performance Tests (if applicable)
**Purpose:** Verify specific non-functional performance requirements.

| Scenario | Target Metric | Acceptance Threshold |
|----------|--------------|---------------------|
| | | |

## 4. Test Cases Mapped to User Stories

| Story | Acceptance Criteria | Test Type | Test Case ID |
|-------|-------------------|-----------|-------------|
| S1 | AC-1 | Unit | TC-001 |
| S1 | AC-2 | Integration | TC-002 |
| S1 | AC-3 | E2E | TC-003 |

## 5. Test Data Requirements

- What test data is needed
- How test data will be provisioned
- Test data cleanup strategy

## 6. Environment Requirements

| Environment | Purpose | Configuration Notes |
|-------------|---------|---------------------|
| Local | Unit + integration tests | |
| Staging | E2E + integration tests | |
| Pre-prod | Performance + E2E | |

## 7. Security Test Considerations (ISO 27001 Awareness)

- [ ] Authentication / authorization boundary testing
- [ ] Input validation and injection prevention
- [ ] Access control verification (least privilege)
- [ ] Sensitive data handling verification
- [ ] Audit log verification

## 8. Test Automation

- CI/CD integration approach
- Test execution triggers
- Flaky test handling strategy
- Test coverage targets

## 9. Risk-Based Testing

| Risk | Test Mitigation | Priority |
|------|----------------|----------|
| [High-risk area] | [Additional test coverage] | |

## 10. Entry and Exit Criteria

### Entry Criteria (ready to test)
- [ ] Code complete and passing unit tests
- [ ] Test environment available
- [ ] Test data provisioned

### Exit Criteria (ready to release)
- [ ] All P0 test cases passing
- [ ] No open P0/P1 defects
- [ ] E2E flows verified (if UI exists)
- [ ] Contract tests passing
- [ ] Edge case and resilience tests passing (concurrency, invalid input, injection, auth errors, network failures)
- [ ] Performance targets met (if applicable)
- [ ] Security checks passed (injection, auth boundary, access control)

## 11. Defect Management

- How defects are reported and tracked
- Severity classification
- Escalation path

## 12. Open Questions

- [ ] [Question 1]
- [ ] [Question 2]
