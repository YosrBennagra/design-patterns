# Decorator

> **Wall Note / A4**
>
> **Intent:** compose optional behavior around a component while preserving its contract. **Strength:** combinations without subclass explosion. **Risk:** hidden wrapper order.

## Detailed Notes

Decorator is explicit composition around an interface.

```java
interface DocumentStore {
    Document get(DocumentId id);
}

record MetricsStore(DocumentStore delegate, Meter meter) implements DocumentStore {
    public Document get(DocumentId id) {
        var timer = meter.start();
        try { return delegate.get(id); }
        finally { timer.stop(); }
    }
}

record CachingStore(DocumentStore delegate, Cache cache) implements DocumentStore {
    public Document get(DocumentId id) {
        return cache.get(id, () -> delegate.get(id));
    }
}
```

### Order matters

```text
Metrics(Caching(Database))
```

measures client-visible latency including cache lookup.

```text
Caching(Metrics(Database))
```

measures only DB calls on cache misses.

That difference must be intentional and tested.

### Decorator vs Proxy
Shape can be identical. Decorator's intent is to **add responsibility**; Proxy's intent is to **control access/stand in for** the subject.

### Spring/Angular
- Spring AOP/proxies can provide decorator-like cross-cutting behavior.
- Angular HTTP interceptors form ordered wrappers around HTTP handling.

Hand-written decorators are often better when ordering/domain semantics should remain explicit.

### Failure modes
- double retries/caches/metrics due to duplicate decoration;
- unclear order;
- wrapper forgets to delegate one method;
- mutable decorator state is not thread-safe;
- equality/identity assumptions break.

## Senior Questions / Exercises
1. Design and test metrics + cache + authorization ordering.
2. When is AOP clearer, and when is explicit Decorator clearer?
3. Decorator vs middleware/filter chain?
4. Which behaviors should never be hidden in a decorator?

## Related Topics
- [Proxy](./proxy.md)
- [Chain of Responsibility](../03-behavioral/chain-of-responsibility.md)
