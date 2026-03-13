# Design by Contract Reference

This reference describes how to apply Design by Contract (DbC) principles within the SDLC workflow.

---

## What is Design by Contract?

Design by Contract defines formal, precise, and verifiable interface specifications for software components. Each contract specifies:

- **Preconditions**: what must be true before a function/service is called
- **Postconditions**: what is guaranteed to be true after the function/service completes
- **Invariants**: what must always be true throughout the lifetime of an object or system

---

## When to apply DbC in the SDLC workflow

Apply Design by Contract specifications when defining:

1. **API endpoints** — request validation (preconditions), response guarantees (postconditions)
2. **Service boundaries** — what a service expects from callers, what it guarantees to callers
3. **Domain rules** — business invariants that must hold across operations
4. **Event schemas** — what an event producer guarantees about the event payload
5. **Integration interfaces** — contracts between bounded contexts or external systems

---

## Contract specification format

Use this format in design documents and architecture confirmations:

```text
Contract: [OperationName](parameters)

Preconditions:
- [what must be true before calling]

Postconditions:
- [what is guaranteed after successful completion]

Invariants:
- [what must always be true]

Error Conditions:
- [condition] -> [error type/response]
```

---

## Mapping contracts to tests

Each part of a contract maps to specific test types:

| Contract Element | Test Type | What to Verify |
|-----------------|-----------|----------------|
| Preconditions | Unit + Integration | Invalid inputs rejected, proper error responses |
| Postconditions | Unit + Integration | Correct state changes, return values, side effects |
| Invariants | Unit + Integration | Invariant holds before and after each operation |
| Error conditions | Unit + Integration | Correct error types, no partial state changes |

When generating test plans, trace each test case back to a specific contract element.

---

## Contract evolution

When contracts change:
1. Document the change in the design doc
2. Assess backward compatibility impact
3. Update consumer tests
4. Consider versioning for external-facing contracts

---

## Common contract patterns

### Idempotent operation
```text
Contract: ProcessPayment(orderId, amount, idempotencyKey)

Preconditions:
- orderId exists
- amount > 0
- idempotencyKey is a valid UUID

Postconditions:
- If first call with this idempotencyKey: payment processed, PaymentProcessed event emitted
- If duplicate idempotencyKey: returns same result as first call, no duplicate processing

Invariants:
- Each idempotencyKey produces at most one side effect
```

### CQRS command
```text
Contract: Command: PlaceOrder(customerId, items[])

Preconditions:
- customerId is authenticated and authorized
- items is non-empty
- all items have available inventory

Postconditions:
- Order aggregate created
- OrderPlaced domain event published
- Inventory reservation created

Invariants:
- Order state is consistent with event history
- No inventory oversold
```

### Event contract
```text
Event: OrderPlaced

Producer guarantees:
- orderId is globally unique
- customerId references an existing customer
- items is non-empty
- totalAmount = sum of (quantity * unitPrice) for all items
- occurredAt is UTC timestamp of when the order was placed

Consumer expectations:
- Event may be delivered more than once (design for idempotency)
- Events arrive in approximate order but not guaranteed strict order
```
