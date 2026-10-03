# Composite

> **Wall Note / A4**
>
> **Intent:** treat individual objects and object trees uniformly. **Signal:** recursive part-whole structures. **Trade-off:** common interface can become too broad.


## Detailed Notes

### What and why
Composite lets clients operate on leaves and containers through the same abstraction. It is natural for trees when operations make sense for both.

### How it works
A component interface defines common operations. Leaves perform them directly; composites delegate or aggregate across children.

### When to use it
Use for file trees, UI trees, organization structures, rule expressions, menus, and nested permissions.

### When NOT to use it
Avoid forcing unrelated operations onto leaves solely to preserve a uniform interface.

### Practical example
A rule engine can model AndRule and OrRule composites plus predicate leaves behind one Rule.evaluate contract.

### Trade-offs
Simplifies recursive client logic; can weaken interface segregation and make type-specific behavior harder.

### Failure modes and common mistakes
Exposing mutable child lists; meaningless methods on leaves; cycles in what should be a tree.

## Senior Questions / Exercises
1. How would you make a Composite immutable?
2. When should clients know leaf vs composite type?
3. What traversal concerns belong in Iterator or Visitor?

## Related Topics
- [Iterator](../03-behavioral/iterator.md)
- [Visitor](../03-behavioral/visitor.md)
