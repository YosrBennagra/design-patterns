# Singleton

> **Wall Note / A4**
>
> **Intent:** guarantee one instance in a defined scope. **Question first:** what scope—thread, request, process, application context, cluster? **Main risk:** hidden global mutable state.

## Detailed Notes

Singleton is often discussed as an implementation trick, but the senior question is **whether uniqueness is a real invariant and in which scope**.

### Spring singleton is not cluster singleton
The default Spring scope means roughly one bean instance **per ApplicationContext**. Ten service replicas normally mean ten instances.

```java
@Service
final class ExchangeRateCalculator {
    // Safe as singleton because behavior is stateless.
    Money convert(Money input, Rate rate) { /* ... */ }
}
```

A mutable field such as `currentUser`, `lastOrder`, or request-specific buffers would be unsafe without explicit concurrency/lifecycle design.

### Plain Java
If true process-level singleton construction is needed, enum is robust:

```java
enum MetricsRegistry {
    INSTANCE;
}
```

But DI is usually preferable because dependencies stay explicit and tests can substitute them.

### Cluster-wide uniqueness
Use distributed coordination, leader election, leases, DB uniqueness/locks, or queue ownership. A class-level singleton cannot enforce cross-process uniqueness.

### When to use
- container-managed stateless shared service;
- immutable configuration/cache component with designed synchronization;
- process-level coordinator when process scope is truly correct.

### Avoid
- global service access via `getInstance()`;
- request/user state;
- shared mutable collections without concurrency policy;
- “singleton” as a substitute for distributed locking.

### Failure modes
- tests influence each other through shared state;
- races on mutable fields;
- hidden dependencies;
- assuming one scheduled job executes once across a cluster.

## Senior Questions / Exercises
1. Define Spring singleton scope precisely.
2. How would you guarantee exactly one cluster leader at a time?
3. Why can a stateless singleton be safe while a mutable one is not?
4. A `@Scheduled` method must run once globally—design it correctly.

## Related Topics
- [Dependency Injection](../04-enterprise/dependency-injection.md)
- [System design](https://github.com/YosrBennagra/system-design)
