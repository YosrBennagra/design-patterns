# Chain of Responsibility

> **Wall Note / A4**
>
> **Intent:** pass a request through an ordered sequence of handlers. **Decide explicitly:** all handlers run, first-match wins, or handlers may short-circuit.

## Detailed Notes

Chain of Responsibility is useful when processing is naturally staged and individual stages should be independently composable.

### Java example

```java
interface OrderCheck {
    CheckResult check(OrderDraft draft);
}

final class ValidationChain {
    private final List<OrderCheck> checks;

    ValidationChain(List<OrderCheck> checks) {
        this.checks = List.copyOf(checks);
    }

    CheckResult run(OrderDraft draft) {
        for (var check : checks) {
            var result = check.check(draft);
            if (!result.accepted()) return result; // explicit short-circuit
        }
        return CheckResult.ok();
    }
}
```

This explicit loop is often clearer than handlers containing mutable `next` pointers.

### Framework examples
- servlet filters;
- Spring Security filter chain;
- Spring MVC interceptors;
- Angular HTTP interceptors.

The frameworks provide ordering/lifecycle; your job is to understand short-circuit and exception semantics.

### Chain vs Pipeline
A pipeline usually assumes every stage transforms/passes output to the next. Chain of Responsibility emphasizes **which handler, if any, handles/terminates the request**. Real middleware often blends both ideas.

### Ordering is part of correctness
Authentication before authorization, decompression before body validation, correlation IDs before logging. Treat order as configuration with tests, not accidental bean discovery.

### Failure modes
- hidden order;
- handler swallows exception and chain continues incorrectly;
- shared mutable request state;
- handler performs expensive work before a cheap rejecting check;
- duplicate handler registration;
- no terminal/default behavior.

## Senior Questions / Exercises
1. How do you make order explicit in Spring?
2. Compare validation chain with one validator class.
3. Design short-circuit semantics for authentication/authorization.
4. Which checks should be ordered first for cost and security?
5. When does a pipeline become easier to reason about than a chain?

## Related Topics
- [Decorator](../02-structural/decorator.md)
- [Command](./command.md)
- [Java/Spring](../07-frameworks/java-spring.md)
