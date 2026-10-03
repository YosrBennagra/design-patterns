# Bridge

> **Wall Note / A4**
>
> **Intent:** vary abstraction and implementation independently. **Signal:** two independent dimensions cause subclass explosion. **Trade-off:** composition/indirection.


## Detailed Notes

### What and why
Bridge replaces a Cartesian-product inheritance hierarchy with two composable hierarchies. It matters when both dimensions genuinely evolve independently.

### How it works
The abstraction holds an implementation interface and delegates implementation-specific work. Each side can then gain variants independently.

### When to use it
Use when UI/control logic varies separately from device/provider/platform implementation, or when two axes multiply subclasses.

### When NOT to use it
Avoid when only one dimension varies or a simple Strategy is enough.

### Practical example
Reporting variants such as SummaryReport and AuditReport can use rendering implementations such as PdfRenderer and HtmlRenderer.

### Trade-offs
Controls subclass explosion and improves substitution; increases collaborating types.

### Failure modes and common mistakes
Introducing Bridge before two variation axes exist; confusing it with Adapter.

## Senior Questions / Exercises
1. Identify the two variation axes in a real codebase before proposing Bridge.
2. Bridge vs Strategy: what is structurally similar and intent-wise different?
3. How can DI assemble a Bridge?

## Related Topics
- [Strategy](../03-behavioral/strategy.md)
- [Adapter](./adapter.md)
