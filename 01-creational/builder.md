# Builder

> **Wall Note / A4**
>
> **Intent:** construct a complex object step by step while keeping valid construction readable. **Use when:** many optional values or construction invariants. **Cost:** extra builder API.


## Detailed Notes

### What and why
Builder makes complex construction explicit and readable. Its strongest form protects invariants by validating at build time rather than merely replacing telescoping constructors.

### How it works
Collect construction parameters in a builder, validate combinations, then create an immutable target. Staged builders can encode mandatory ordering in types but should be used sparingly.

### When to use it
Use for immutable configuration objects, test-data construction, domain objects with many optional values, and APIs where named construction is clearer than long parameter lists.

### When NOT to use it
Avoid when a constructor or static factory with a few parameters is already clear. Do not use Builder to mask a target object with too many responsibilities.

### Practical example
A ReportRequest with filters, format, locale, sorting, and date range can benefit from a builder.

### Trade-offs
Readable calls and centralized validation; more code and potential duplication. Mutable builders should not be shared across threads.

### Failure modes and common mistakes
Allowing invalid final states; putting workflows in the builder; blindly generating builders on entities with invariants.

## Senior Questions / Exercises
1. When is a static factory clearer than Builder?
2. Design a builder that guarantees two mandatory fields before build.
3. What problems arise from builders on JPA entities?

## Related Topics
- [Factory Method](./factory-method.md)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
