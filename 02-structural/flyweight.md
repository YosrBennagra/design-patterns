# Flyweight

> **Wall Note / A4**
>
> **Intent:** share intrinsic immutable state across many fine-grained objects. **Signal:** proven duplicate-state memory pressure. **Trade-off:** externalized context.


## Detailed Notes

### What and why
Flyweight is a memory optimization. It separates shareable intrinsic state from per-use extrinsic state. Measure before applying it.

### How it works
Cache immutable flyweights by key and pass contextual data to operations rather than storing it per instance.

### When to use it
Use in rendering, parsers, compilers, games, or very high-cardinality graphs with measured duplicate immutable state.

### When NOT to use it
Avoid without profiling evidence or when shared state is mutable.

### Practical example
Text rendering can share glyph metadata while position and color remain extrinsic per occurrence.

### Trade-offs
Can dramatically reduce memory; increases conceptual complexity and lookup overhead.

### Failure modes and common mistakes
Sharing mutable objects; unbounded flyweight caches; optimizing allocations the JVM already handles efficiently.

## Senior Questions / Exercises
1. What profiling evidence would justify Flyweight?
2. How do interning and caches relate to Flyweight?
3. What makes a flyweight safe across threads?

## Related Topics
- [Computer science fundamentals](https://github.com/YosrBennagra/computer-science-fundamentals)
- [Prototype](../01-creational/prototype.md)
