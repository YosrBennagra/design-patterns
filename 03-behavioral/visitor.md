# Visitor

> **Wall Note / A4**
>
> **Intent:** add many operations over a stable heterogeneous structure. **Great when:** element types are stable, operations grow. **Bad when:** new element types are frequent.

## Detailed Notes

Visitor moves operations out of element classes using double dispatch.

### Classic shape

```java
sealed interface Expr permits NumberExpr, AddExpr {
    <R> R accept(ExprVisitor<R> visitor);
}

record NumberExpr(int value) implements Expr {
    public <R> R accept(ExprVisitor<R> v) { return v.visitNumber(this); }
}

record AddExpr(Expr left, Expr right) implements Expr {
    public <R> R accept(ExprVisitor<R> v) { return v.visitAdd(this); }
}

interface ExprVisitor<R> {
    R visitNumber(NumberExpr n);
    R visitAdd(AddExpr a);
}
```

Add evaluators, printers, type checkers, code generators as separate visitors.

### Modern Java alternative
With sealed hierarchies and exhaustive pattern matching, a function using `switch` can be much simpler. Visitor remains useful when:
- operations are numerous and deserve separate types;
- compatibility with classic OO dispatch matters;
- language/version constraints limit pattern matching.

### Visitor + Composite
Composite gives the tree; Visitor gives operations over element variants.

### Evolution trade-off
Adding a new operation → add one visitor, elements unchanged.
Adding a new element type → every visitor must change.

This is the inverse pressure of putting methods on element types.

### Failure modes
- element hierarchy changes weekly;
- visitor becomes mutable shared bag of state;
- generic reflection used instead of explicit dispatch;
- business behavior that belongs on the domain object is moved out merely to “use Visitor.”

## Senior Questions / Exercises
1. Visitor vs sealed-class pattern matching in modern Java?
2. Which change axis is optimized?
3. Design type-check + render operations for an AST.
4. How would you carry traversal context without unsafe mutable visitor state?

## Related Topics
- [Composite](../02-structural/composite.md)
- [Iterator](./iterator.md)
- [Computer science fundamentals](https://github.com/YosrBennagra/computer-science-fundamentals)
