# DTO & Mapper

> **Wall Note / A4**
>
> **DTO:** data shape for a boundary. **Mapper:** explicit translation. **Why:** prevent transport/persistence/domain coupling. **Risk:** pointless duplication/mega-mappers.


## Detailed Notes

### What and why
Different boundaries evolve for different reasons. DTOs let API contracts, domain models, and persistence representations vary independently when needed.

### How it works
Keep mapping near boundaries, make semantic conversions explicit, validate inputs before mapping, and avoid reflection-heavy magic for complex conversions.

### When to use it
Use at external APIs, messaging boundaries, integration adapters, and wherever exposing internal entities creates coupling/security issues.

### When NOT to use it
Avoid duplicating identical internal value shapes when there is no boundary or evolution pressure.

### Practical example
Map CreateOrderRequest to an application command and map a read model to OrderResponse; never bind a JPA entity directly as an HTTP request body.

### Trade-offs
Boundary clarity and security; mapping code and drift require maintenance.

### Failure modes and common mistakes
Mass assignment, leaking internal fields, N+1 during serialization, bidirectional mega-mappers.

## Senior Questions / Exercises
1. Why is returning JPA entities from controllers risky?
2. When can mapping be intentionally one-way?
3. How do versioned API DTOs protect evolution?

## Related Topics
- [Adapter](../02-structural/adapter.md)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
