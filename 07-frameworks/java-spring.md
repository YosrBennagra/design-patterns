# Java & Spring Pattern Map

> **Wall Note / A4**
>
> Spring already implements many pattern mechanics. Know the underlying intent, then use the framework instead of rebuilding it.

## Detailed Notes

| Mechanism | Pattern ideas |
|---|---|
| constructor injection / bean wiring | Dependency Injection, Factory |
| BeanFactory / ApplicationContext | Factory + registry/container; avoid service-locator usage in domain code |
| bean scopes | lifecycle management; singleton is per context, not per cluster |
| transactional advice, AOP, method security | Proxy / interception / decorator-like behavior |
| JdbcTemplate and transaction templates | template/callback family |
| Spring Data repositories | Repository with real query semantics |
| application events | Observer; in-process, not durable messaging |
| HTTP client wrappers | Adapter / Facade |
| multiple beans implementing one interface | Strategy via DI |
| filter chains | Chain of Responsibility |

### Framework-provided vs hand-written

| Problem | Prefer first | Hand-write when |
|---|---|---|
| object wiring | constructor injection | special runtime creation semantics are required |
| cross-cutting transaction/security | Spring proxy/AOP | semantics must be explicit or proxy limitations are harmful |
| database access | Spring Data/JdbcTemplate/JPA | abstraction needs domain-specific semantics not supplied by framework |
| provider policy selection | injected Strategy beans | selection is dynamic enough to require a dedicated registry/factory |
| in-process events | application events | you need a more explicit typed event mechanism |
| durable cross-service events | broker + outbox | never replace durability with Spring in-process events |

### Critical proxy facts
- Advice usually applies when calls pass through the proxy.
- Self-invocation may bypass advice.
- Proxy strategy and final methods/classes can matter.
- A local transaction does not create a distributed transaction across services.
- Singleton-scoped beans should generally be stateless or explicitly thread-safe.

### Strategy through DI

```java
public interface TaxStrategy {
    Tax quote(Order order);
}

@Component("standard")
final class StandardTaxStrategy implements TaxStrategy {
    public Tax quote(Order order) { /* ... */ }
}

@Service
final class TaxService {
    private final Map<String, TaxStrategy> strategies;

    TaxService(Map<String, TaxStrategy> strategies) {
        this.strategies = Map.copyOf(strategies);
    }

    Tax quote(String policy, Order order) {
        var strategy = strategies.get(policy);
        if (strategy == null) throw new IllegalArgumentException("Unknown policy");
        return strategy.quote(order);
    }
}
```

### Adapter at an external boundary

```java
public interface FraudCheck {
    FraudDecision evaluate(Order order);
}

@Component
final class VendorFraudAdapter implements FraudCheck {
    private final VendorClient vendor;

    VendorFraudAdapter(VendorClient vendor) {
        this.vendor = vendor;
    }

    public FraudDecision evaluate(Order order) {
        var response = vendor.score(order.id().toString(), order.total().minorUnits());
        return FraudDecision.fromScore(response.score());
    }
}
```

Vendor types stop at the adapter.

### Transaction boundary

```java
@Service
final class PlaceOrderService {
    private final OrderRepository orders;
    private final OutboxRepository outbox;

    @Transactional
    public OrderId place(PlaceOrder command) {
        var order = Order.create(command);
        orders.save(order);
        outbox.append(OrderPlaced.from(order));
        return order.id();
    }
}
```

The transaction owns database state plus the outbox record. It does **not** call the remote payment provider inside the same transaction.

### Decision checklist
Before creating a class named Factory, Manager, Handler, Provider, or Strategy:
1. What concrete force is changing?
2. Is the framework already solving the mechanism?
3. Can a function/map/composition solve it more simply?
4. Where is lifetime/transaction/concurrency ownership?
5. Can a teammate trace the call path quickly?
6. What behavior must integration tests prove?

## Senior Questions / Exercises
1. Explain why a Spring singleton is not a distributed singleton.
2. Explain a transactional self-invocation failure.
3. Choose between hand-written Decorator and Spring AOP for metrics.
4. Refactor provider selection using Spring DI without a service locator.
5. When does a Spring Data repository abstraction become harmful?
6. Distinguish Spring application events from durable integration events.
7. Identify three cases where a GoF pattern exists conceptually but should not be hand-written in Spring.

## Related Topics
- [Proxy](../02-structural/proxy.md)
- [Strategy](../03-behavioral/strategy.md)
- [Adapter](../02-structural/adapter.md)
- [Dependency Injection](../04-enterprise/dependency-injection.md)
- [Repository & Unit of Work](../04-enterprise/repository-unit-of-work.md)
- [System design](https://github.com/YosrBennagra/system-design)
