# Dependency Injection

> **Wall Note / A4**
>
> **Intent:** collaborators are supplied from outside instead of constructed/located internally. **Result:** explicit dependency graph, replaceable policies, managed lifecycle. **Risk:** container magic and oversized graphs.

## Detailed Notes

DI separates **object behavior** from **object assembly**.

### Prefer constructor injection

```java
@Service
final class CheckoutService {
    private final OrderRepository orders;
    private final PaymentGateway payments;

    CheckoutService(OrderRepository orders, PaymentGateway payments) {
        this.orders = orders;
        this.payments = payments;
    }
}
```

Benefits:
- dependencies cannot be forgotten;
- fields can be final;
- class can be constructed in an ordinary unit test;
- dependency count is visible.

### Composition root
Keep implementation selection, credentials, scopes, and environment-specific wiring at the application boundary/configuration layer. Domain code should not ask `ApplicationContext` for services.

### Qualifiers and multiple implementations
Multiple beans are healthy when they represent a real variation axis. Prefer typed qualifiers, maps, or explicit configuration over repeated string lookups spread through business code.

### Lifecycle and scope
Injection also carries lifecycle semantics:
- singleton bean: shared within one context;
- request/session scope: context-specific lifetime;
- prototype: new bean when requested from container, with lifecycle caveats.

Never store request/user-specific mutable state in an ordinary singleton service.

### Circular dependency as design feedback
A ↔ B often signals:
- responsibilities are mixed;
- orchestration boundary is missing;
- shared concept deserves extraction;
- event/callback is being used to avoid a clearer direction.

Do not treat circular-dependency flags as an inconvenience to disable.

### DI vs service locator
```java
var payment = context.getBean(PaymentGateway.class);
```
inside application/domain logic hides the dependency. That is Service Locator behavior even though Spring provides the lookup.

### Angular
Injection tokens are especially useful for browser/platform boundaries and configuration. Keep components dependent on feature/application abstractions rather than directly on vendor SDKs.

### Failure modes
- field injection;
- 15 constructor dependencies signaling poor cohesion;
- circular graph patched with lazy injection;
- mutable singleton state;
- injecting repositories/providers directly into every layer;
- domain objects coupled to the DI container.

## Senior Questions / Exercises
1. Why is constructor injection usually superior to field injection?
2. What does a 12-dependency constructor tell you?
3. Explain Spring singleton scope vs process/cluster scope.
4. Refactor a service-locator call into explicit DI.
5. When is injecting `List<Strategy>` better than a factory?

## Related Topics
- [Singleton](../01-creational/singleton.md)
- [Strategy](../03-behavioral/strategy.md)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
