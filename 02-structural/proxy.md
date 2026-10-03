# Proxy

> **Wall Note / A4**
>
> **Intent:** control access to another object while preserving its interface. **Signal:** lazy loading, remote access, authorization, caching, instrumentation. **Trade-off:** hidden behavior.


## Detailed Notes

### What and why
Proxy stands in for a real subject and controls access. Remote, virtual, protection, and smart-reference proxies share structure but differ in purpose.

### How it works
Implement the same interface and delegate conditionally. Framework proxies may be dynamic and interception-based.

### When to use it
Use for lazy loading, remote clients, authorization gates, transaction/interception boundaries, and framework-generated Spring proxies.

### When NOT to use it
Avoid hiding expensive remote calls behind an interface that looks local unless latency and failure semantics are explicit.

### Practical example
Spring transactional advice commonly works through a proxy; self-invocation can bypass the proxy.

### Trade-offs
Centralizes access concerns; can surprise callers with latency, identity, or lifecycle behavior.

### Failure modes and common mistakes
Self-invocation; equals/hashCode surprises; lazy loading outside persistence context; unsupported final methods/classes.

## Senior Questions / Exercises
1. Why can transactional advice fail on self-invocation?
2. Proxy vs Decorator: compare intent and transparency.
3. How should a remote proxy expose network failure semantics?

## Related Topics
- [Decorator](./decorator.md)
- [Java/Spring examples](../07-frameworks/java-spring.md)
