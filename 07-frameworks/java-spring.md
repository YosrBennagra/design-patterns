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

### Critical proxy facts
- Advice usually applies when calls pass through the proxy.
- Self-invocation may bypass advice.
- Proxy strategy and final methods/classes can matter.
- A local transaction does not create a distributed transaction across services.
- Singleton-scoped beans should generally be stateless or explicitly thread-safe.

### Practical mini-example

```java
public interface TaxStrategy {
    Tax quote(Order order);
}

@Component("standard")
final class StandardTaxStrategy implements TaxStrategy {
    // ...
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

The important design is isolating policy variation without a giant selection switch.

## Senior Questions / Exercises
1. Explain why a Spring singleton is not a distributed singleton.
2. Explain a transactional self-invocation failure.
3. Choose between hand-written Decorator and Spring AOP for metrics.
4. Refactor provider selection using Spring DI without a service locator.
5. When does a Spring Data repository abstraction become harmful?

## Related Topics
- [Proxy](../02-structural/proxy.md)
- [Strategy](../03-behavioral/strategy.md)
- [Dependency Injection](../04-enterprise/dependency-injection.md)
- [System design](https://github.com/YosrBennagra/system-design)
