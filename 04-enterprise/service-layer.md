# Service Layer

> **Wall Note / A4**
>
> **Intent:** define application use-case boundaries and coordinate domain/infrastructure work. **Keep:** orchestration here, domain rules in domain objects/policies. **Risk:** god service/anemic domain.


## Detailed Notes

### What and why
A service layer gives clients a stable entry point to use cases and is a natural place for transaction/authorization boundaries and orchestration.

### How it works
Expose task-oriented methods, coordinate repositories/domain/external ports, and keep transport concerns outside.

### When to use it
Use in business applications with multiple entry points or non-trivial transactions/workflows.

### When NOT to use it
Avoid one service with hundreds of unrelated methods or pass-through CRUD services.

### Practical example
PlaceOrderService may load customer/cart, invoke domain behavior, persist aggregates, and write an outbox record.

### Trade-offs
Clarifies use cases and boundaries; can become procedural/anemic if all rules move into services.

### Failure modes and common mistakes
God services, transaction per helper method, HTTP concerns in application services, hidden remote calls.

## Senior Questions / Exercises
1. Application service vs domain service—what belongs where?
2. Where should transactional boundaries usually be placed?
3. How do you avoid a distributed transaction?

## Related Topics
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
- [System design](https://github.com/YosrBennagra/system-design)
