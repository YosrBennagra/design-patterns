# Chain of Responsibility

> **Wall Note / A4**
>
> **Intent:** pass a request through ordered handlers. **Signal:** variable processing pipeline. **Trade-off:** order/completion can become implicit.


## Detailed Notes

### What and why
Chain of Responsibility decouples senders from the exact handler sequence. It fits middleware-like pipelines where handlers can process, enrich, reject, or pass on.

### How it works
Handlers implement a common contract and invoke the next handler, or a framework composes them. Decide whether all handlers run or first-match wins.

### When to use it
Use for HTTP filters/interceptors, validation pipelines, approval chains, security filters, and event stages.

### When NOT to use it
Avoid when processing order must be obvious in one place and the chain makes control flow harder to understand.

### Practical example
Spring Security filter chains and servlet filters are practical variants; Angular HTTP interceptors form a client-side chain.

### Trade-offs
Composable and extensible; ordering, short-circuiting, and error propagation need explicit rules.

### Failure modes and common mistakes
Uncontrolled ordering; hidden shared state; swallowed errors; missing terminal behavior.

## Senior Questions / Exercises
1. How would you make handler order explicit and testable?
2. When is Pipeline a better model than Chain of Responsibility?
3. How do Spring Security filters illustrate short-circuiting?

## Related Topics
- [Decorator](../02-structural/decorator.md)
- [Command](./command.md)
