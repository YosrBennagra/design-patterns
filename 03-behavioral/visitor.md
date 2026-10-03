# Visitor

> **Wall Note / A4**
>
> **Intent:** add operations over a stable object structure without modifying element classes. **Signal:** many operations over a stable type hierarchy. **Trade-off:** new element types are expensive.


## Detailed Notes

### What and why
Visitor uses double dispatch to move operations out of element classes. It is powerful when element types are stable and operations change frequently.

### How it works
Each element accepts a visitor and calls the overload for its concrete type. Modern sealed types/pattern matching can sometimes be simpler.

### When to use it
Use for ASTs, compilers, document models, and stable heterogeneous structures needing many operations.

### When NOT to use it
Avoid when element types change frequently or pattern matching over sealed hierarchies is clearer.

### Practical example
An AST can support type checking, code generation, and pretty printing as separate visitors.

### Trade-offs
Keeps operations cohesive and elements focused; couples every visitor to the full element set.

### Failure modes and common mistakes
Visitor with constantly changing element types; reflection instead of clear dispatch; visitors with unpredictable side effects.

## Senior Questions / Exercises
1. Why is Visitor good for stable element types?
2. How do Java sealed classes and pattern matching change the trade-off?
3. Visitor vs Strategy: what varies?

## Related Topics
- [Composite](../02-structural/composite.md)
- [Computer science fundamentals](https://github.com/YosrBennagra/computer-science-fundamentals)
