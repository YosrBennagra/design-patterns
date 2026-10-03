# Composite

> **Wall Note / A4**
>
> **Intent:** treat leaves and containers uniformly in a recursive part-whole structure. **Risk:** forcing meaningless operations onto every component.

## Detailed Notes

Composite fits true trees/recursive structures.

### Example — rule tree

```java
sealed interface Rule permits PredicateRule, AndRule, OrRule {
    boolean evaluate(Context context);
}

record PredicateRule(Predicate<Context> predicate) implements Rule {
    public boolean evaluate(Context c) { return predicate.test(c); }
}

record AndRule(List<Rule> children) implements Rule {
    AndRule { children = List.copyOf(children); }
    public boolean evaluate(Context c) {
        return children.stream().allMatch(r -> r.evaluate(c));
    }
}
```

The client evaluates one `Rule` whether it is a leaf or a subtree.

### Design concerns
- prefer immutable child collections where possible;
- prevent cycles if semantics require a tree;
- define traversal order;
- consider stack depth for very deep trees;
- keep the common interface narrow.

### Composite + Iterator/Visitor
Composite represents the structure. Iterator controls traversal. Visitor adds operations over a stable heterogeneous tree. Do not put every possible operation into the component interface.

### Failure modes
- exposing mutable internal children;
- parent/child ownership unclear;
- a “tree” can contain cycles and recursive methods never terminate;
- component interface bloated with leaf-only/composite-only operations.

## Senior Questions / Exercises
1. Make a Composite immutable and thread-safe.
2. How would you detect/prevent cycles?
3. When should traversal move to Iterator?
4. Compare Composite with a simple recursive data structure plus pattern matching.

## Related Topics
- [Iterator](../03-behavioral/iterator.md)
- [Visitor](../03-behavioral/visitor.md)
