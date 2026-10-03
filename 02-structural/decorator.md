# Decorator

> **Wall Note / A4**
>
> **Intent:** add behavior around an object dynamically while preserving its interface. **Signal:** optional behavior combinations. **Trade-off:** wrapper chains.


## Detailed Notes

### What and why
Decorator composes behavior without subclass explosion. It is effective for orthogonal concerns that surround a core operation.

### How it works
A decorator implements the same interface, stores a delegate, performs behavior before/after delegation, and can be stacked.

### When to use it
Use for metrics, caching, authorization checks, instrumentation, stream processing, Angular interceptors, and Spring proxy-like cross-cutting behavior.

### When NOT to use it
Avoid deep invisible chains where order changes semantics unexpectedly, or when middleware/AOP is clearer.

### Practical example
Wrap DocumentStore with MetricsDocumentStore and CachingDocumentStore. Spring AOP often provides proxy-based decoration.

### Trade-offs
Highly composable and open/closed; debugging order and identity can be harder.

### Failure modes and common mistakes
Order bugs; double application; stateful non-thread-safe decorators; confusing Decorator with inheritance.

## Senior Questions / Exercises
1. Decorator vs Proxy: same shape, different intent—explain.
2. How would you test ordering of multiple decorators?
3. When should Spring AOP replace hand-written decorators?

## Related Topics
- [Proxy](./proxy.md)
- [Chain of Responsibility](../03-behavioral/chain-of-responsibility.md)
