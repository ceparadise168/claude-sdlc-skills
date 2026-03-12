# ASCII Diagram Conventions

This reference defines the ASCII diagram styles to use when confirming architecture, flows, and domain models with the user.

---

## General principles

- Keep diagrams compact and readable in plain text
- Use consistent box styles throughout a single diagram
- Label all arrows with the interaction or data being passed
- Highlight uncertain areas with `[?]` or `(assumed)` annotations
- Always accompany diagrams with a brief interpretation

---

## 1. Component / Architecture View

Use box-and-arrow diagrams to show system components and their relationships.

```text
+-------------------+       +-------------------+
|   Web / Mobile    | ----> |   API Gateway     |
+-------------------+       +-------------------+
                                      |
                                      v
                           +-------------------+
                           |  Order Service    |
                           +-------------------+
                                      |
                    +-----------------+-----------------+
                    v                                   v
          +-------------------+              +-------------------+
          |  Order DB         |              |  Payment Service  |
          +-------------------+              +-------------------+
```

### Conventions:
- `+---+` for component boxes
- `---->` for synchronous calls
- `- - >` for asynchronous / event-driven
- `|` for vertical connections
- Label boxes with service/component names

---

## 2. Sequence Diagram

Use UML-like participant columns with message arrows.

```text
User        App        API        OrderSvc       PaymentSvc     DB
 |           |          |             |              |           |
 | Place Order          |             |              |           |
 |---------> |          |             |              |           |
 |           | POST /orders           |              |           |
 |           |--------> |             |              |           |
 |           |          | Create Order|              |           |
 |           |          |-----------> |              |           |
 |           |          |             | Charge        |           |
 |           |          |             |------------> |           |
 |           |          |             |              | Write     |
 |           |          |             |              |-------->  |
 |           |          |             |   OK         |           |
 |           |          |             |<------------ |           |
 |           |          |  Created    |              |           |
 |           |          |<----------- |              |           |
 |           | 201      |             |              |           |
 |           |<-------- |             |              |           |
 | Success   |          |             |              |           |
 |<--------- |          |             |              |           |
```

### Conventions:
- Align participant names at the top
- Use `|` for lifelines
- `-------->` for request arrows (label above)
- `<--------` for response arrows (label above)
- Read top-to-bottom as time progression

---

## 3. Flowchart

Use decision diamonds `{}` and process boxes `[]`.

```text
[Start]
   |
   v
[Receive Request]
   |
   v
{Valid Input?} -- No --> [Return 400 Bad Request]
   |
  Yes
   |
   v
[Load Context]
   |
   v
{Authorized?} -- No --> [Return 403 Forbidden]
   |
  Yes
   |
   v
[Process Business Logic]
   |
   v
{Success?} -- No --> [Return 500 + Log Error]
   |
  Yes
   |
   v
[Return 200 + Emit Event]
   |
   v
[End]
```

### Conventions:
- `[Process]` for action steps
- `{Decision?}` for decision points
- `-- Label -->` for branching
- Vertical flow for the happy path

---

## 4. Domain Model Sketch

Show entities, aggregates, and their relationships.

```text
+----------------------+
| <<Aggregate Root>>   |
| Order                |
| - orderId            |
| - status             |
| - totalAmount        |
+----------------------+
           |
           | 1..*
           v
+----------------------+
| OrderItem            |
| - productId          |
| - quantity           |
| - unitPrice          |
+----------------------+

+----------------------+
| <<Value Object>>     |
| Money                |
| - amount             |
| - currency           |
+----------------------+
```

### Conventions:
- `<<Aggregate Root>>` annotation for aggregate roots
- `<<Value Object>>` annotation for value objects
- `1..*` or `0..1` for cardinality
- Group related entities visually

---

## 5. Bounded Context Map

Show DDD bounded contexts and their relationships.

```text
+========================+       +========================+
||   Order Context       ||      ||  Payment Context      ||
||                       || ---> ||                       ||
|| - Order               ||      || - Payment             ||
|| - OrderItem           ||      || - PaymentMethod       ||
|| - OrderService        ||      || - PaymentGateway      ||
+========================+       +========================+
           |
           | publishes OrderPlaced event
           v
+========================+
||  Notification Context ||
||                       ||
|| - NotificationService ||
|| - Template            ||
+========================+
```

### Conventions:
- `+====+` double border for bounded contexts
- `--->` for synchronous integration
- `publishes [Event]` for event-driven integration
- List key domain objects inside each context

---

## 6. Clean Architecture Layers

```text
+-----------------------------+
| Interface Adapters          |
| REST Controller / GraphQL   |
| CLI / Event Handler         |
+-----------------------------+
              |
              v
+-----------------------------+
| Application Layer           |
| Use Cases / Command Handler |
| DTOs / Application Services |
+-----------------------------+
              |
              v
+-----------------------------+
| Domain Layer                |
| Entities / Aggregates       |
| Domain Services / Events    |
| Value Objects / Policies    |
+-----------------------------+
              |
              v
+-----------------------------+
| Infrastructure              |
| Repository Implementations  |
| Message Queue Adapters      |
| External API Clients        |
+-----------------------------+
```

### Conventions:
- Dependencies point inward (top to bottom)
- Each layer lists its typical inhabitants
- Customize inhabitants for the specific feature

---

## 7. Design by Contract Interface Block

```text
Contract: CreateOrder(customerId, items[], shippingAddress)

Preconditions:
- customerId exists and is active
- items is non-empty
- each item has quantity > 0 and valid productId
- shippingAddress is complete and validated

Postconditions:
- Order is created with status = PENDING
- OrderItem records created for each item
- OrderCreated event emitted
- Inventory reserved for each item

Invariants:
- Order total = sum(item.quantity * item.unitPrice) for all items
- Order always has at least one item
- orderId is globally unique

Error Conditions:
- Invalid customerId -> CustomerNotFound error
- Invalid productId -> ProductNotFound error
- Insufficient inventory -> InsufficientStock error
```

---

## 8. State Transition Diagram

```text
                    create
[*] --------------------------------> [PENDING]
                                          |
                              pay         |        cancel
                    +------------------+  |  +------------------+
                    v                  |  |  v                  |
               [CONFIRMED] <----------+  +-------> [CANCELLED]
                    |
                    | ship
                    v
               [SHIPPED]
                    |
                    | deliver
                    v
               [DELIVERED]
```

### Conventions:
- `[*]` for initial state
- `[STATE_NAME]` for states
- Label transitions on arrows
- Show both happy path and alternative transitions

---

## 9. Deployment View

```text
+------------------+     +------------------+     +------------------+
|  Load Balancer   | --> |  App Server x3   | --> |  PostgreSQL      |
|  (nginx/ALB)     |     |  (Docker/K8s)    |     |  (RDS/managed)   |
+------------------+     +------------------+     +------------------+
                                   |
                                   v
                          +------------------+
                          |  Redis Cache     |
                          |  (ElastiCache)   |
                          +------------------+
                                   |
                                   v
                          +------------------+
                          |  Message Queue   |
                          |  (SQS/RabbitMQ)  |
                          +------------------+
```

### Conventions:
- Show infrastructure components with technology choices
- Label scaling (e.g., "x3")
- Show data flow direction
