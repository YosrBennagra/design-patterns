# DTO & Mapper

> **Wall Note / A4**
>
> **DTO:** boundary-specific data contract. **Mapper:** semantic translation between models. **Why:** API/domain/persistence/integration models evolve for different reasons. **Risk:** blind duplication and mega-mappers.

## Detailed Notes

Do not expose JPA entities or vendor SDK models simply because their fields currently resemble the API.

### HTTP boundary example

```java
public record CreateOrderRequest(
    UUID customerId,
    List<LineRequest> lines
) {}

public record PlaceOrder(
    CustomerId customerId,
    List<OrderLineInput> lines
) {}

final class OrderHttpMapper {
    PlaceOrder toCommand(CreateOrderRequest request) {
        return new PlaceOrder(
            new CustomerId(request.customerId()),
            request.lines().stream().map(this::toLine).toList()
        );
    }
}
```

The mapper is allowed to perform **representation conversion**, not hidden business workflows.

### Why not return JPA entities?
- lazy relationships can trigger N+1 during serialization;
- persistence fields leak;
- bidirectional graphs recurse;
- clients become coupled to DB model;
- mass assignment becomes easier;
- changing schema can accidentally change API.

### One-way mapping is healthy
A create request often maps to a command; there is no reason to map that command back to the same request type. Avoid generic “bidirectional mapper” abstractions when semantics are asymmetric.

### API versioning
`OrderResponseV1` and `OrderResponseV2` can coexist while domain/persistence stay stable. Boundary DTOs provide evolution room.

### Integration events
Use explicit event contracts. Do not serialize domain/JPA objects directly onto a broker.

### Generated mappers
Tools such as MapStruct can reduce mechanical field copying. Keep custom semantic conversions explicit and tested; generation should not hide security/defaulting rules.

### Failure modes
- mega-mapper with hundreds of unrelated types;
- reflection magic silently ignores fields;
- mapper performs repository/network lookups;
- DTO contains internal flags clients can mass-assign;
- lazy entity graph serialization;
- version changes coupled across all layers.

## Senior Questions / Exercises
1. Why is returning JPA entities risky even if JSON looks correct today?
2. When should API and domain models intentionally differ?
3. Map a versioned request into one stable application command.
4. What belongs in a mapper vs domain factory/service?
5. How do you test that sensitive fields never leak into response DTOs?

## Related Topics
- [Adapter](../02-structural/adapter.md)
- [Service Layer](./service-layer.md)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
