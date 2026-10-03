# Service Layer

> **Wall Note / A4**
>
> **Intent:** expose application use cases and coordinate transactions, domain objects, repositories, and external ports. **Keep rules close to their owner.** **Risk:** god service + anemic domain.

## Detailed Notes

A Service Layer/application service answers: **what can the application do?**

### Good shape

```java
@Service
final class PlaceOrderService {
    private final OrderRepository orders;
    private final CustomerRepository customers;
    private final OutboxRepository outbox;

    @Transactional
    OrderId place(PlaceOrder cmd) {
        var customer = customers.get(cmd.customerId());
        var order = Order.place(customer, cmd.lines());
        orders.save(order);
        outbox.append(OrderPlaced.from(order));
        return order.id();
    }
}
```

The service:
- defines the use-case transaction boundary;
- loads collaborators/aggregates;
- invokes domain behavior;
- persists effects;
- coordinates durable integration.

It should not contain every pricing, eligibility, transition, or validation rule.

### Application service vs domain service
**Application service:** orchestration/use case/transaction boundary.
**Domain service:** domain logic that does not naturally belong to one entity/value object.

### Remote calls and transactions
Avoid holding a DB transaction open across slow external calls. Prefer:
- commit local intent/state;
- outbox/workflow;
- explicit pending state;
- idempotent external operation;
- later reconciliation.

### Authorization
Application/use-case boundary is a good place for task-level authorization, while entities/policies may enforce business invariants independent of caller identity.

### Task-oriented API
Prefer:
- `placeOrder`
- `approveInvoice`
- `reserveInventory`

over universal:
- `save(entity)`
- `update(dto)`

when the domain has meaningful workflows.

### Failure modes
- one `OrderService` with 150 methods;
- CRUD pass-through layer adding no semantics;
- HTTP request/response objects in application layer;
- transaction on every private helper;
- external SDK called directly everywhere;
- domain entities reduced to getters/setters.

## Senior Questions / Exercises
1. Application service vs domain service?
2. Where should a transaction start/end?
3. How do you handle external payment without holding the DB transaction?
4. Refactor a 2,000-line service into cohesive use cases/domain policies.
5. When is a thin CRUD service acceptable?

## Related Topics
- [Repository & Unit of Work](./repository-unit-of-work.md)
- [Facade](../02-structural/facade.md)
- [System design: transactions](https://github.com/YosrBennagra/system-design)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
