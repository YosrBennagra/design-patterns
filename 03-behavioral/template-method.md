# Template Method

> **Wall Note / A4**
>
> **Intent:** define an algorithm skeleton with overridable steps. **Signal:** workflows are mostly identical with controlled variation. **Trade-off:** inheritance coupling.


## Detailed Notes

### What and why
Template Method reuses workflow structure through inheritance. It can be effective in frameworks, while composition is often more flexible in application code.

### How it works
A base class defines a sequence and protected hooks/abstract steps. Subclasses customize selected points.

### When to use it
Use when a framework controls lifecycle and extension points are deliberate and stable.

### When NOT to use it
Avoid deep inheritance, fragile hooks, or cases where Strategy/composition expresses variation more locally.

### Practical example
Spring template classes such as JdbcTemplate represent the broader template/callback family: stable resource/error workflow with customizable work.

### Trade-offs
Centralizes invariant workflow; inheritance makes variation less flexible and can create fragile base classes.

### Failure modes and common mistakes
Calling overridable methods from constructors; too many hooks; subclasses bypassing required steps.

## Senior Questions / Exercises
1. Template Method vs Strategy: when does composition win?
2. What makes a hook safe and stable?
3. Identify a fragile-base-class risk.

## Related Topics
- [Strategy](./strategy.md)
- [Java/Spring examples](../07-frameworks/java-spring.md)
