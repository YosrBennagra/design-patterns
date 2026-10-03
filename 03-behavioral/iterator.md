# Iterator

> **Wall Note / A4**
>
> **Intent:** traverse a collection without exposing representation. **Signal:** multiple traversal policies. **Trade-off:** traversal lifecycle/state.


## Detailed Notes

### What and why
Iterator separates traversal from container representation. Modern languages often hide the classic pattern behind iterable and stream abstractions.

### How it works
Expose an iterator/cursor that tracks traversal state. For remote/paginated data, make continuation tokens and resource lifecycle explicit.

### When to use it
Use for custom collections, lazy traversal, graph/tree traversal, streaming, and cursor-like APIs.

### When NOT to use it
Avoid custom iterators when standard collection/stream APIs already fit.

### Practical example
A tree can expose depth-first and breadth-first iterators without leaking node storage details.

### Trade-offs
Uniform traversal and laziness; concurrent modification and resource closure can complicate semantics.

### Failure modes and common mistakes
Holding database resources too long; assuming repeatability; mutating during traversal without defined behavior.

## Senior Questions / Exercises
1. Iterator vs Java Stream: what concerns differ?
2. How would you model cursor pagination safely?
3. What does fail-fast iteration protect against?

## Related Topics
- [Composite](../02-structural/composite.md)
- [Computer science fundamentals](https://github.com/YosrBennagra/computer-science-fundamentals)
