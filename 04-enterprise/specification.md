# Specification

> **Wall Note / A4**
>
> **Intent:** represent a composable business predicate/rule. **Use when:** rules need reuse, composition, explanation, or query translation. **Risk:** unreadable generic rule frameworks.


## Detailed Notes

### What and why
Specification makes meaningful business predicates explicit and composable instead of scattering Boolean conditions.

### How it works
Expose isSatisfiedBy(candidate) and optionally and/or/not. Separate domain specifications from persistence-specific query criteria when translation becomes leaky.

### When to use it
Use for reusable eligibility rules, filtering policies, and complex named predicates.

### When NOT to use it
Avoid wrapping one-off conditions or building a universal rules DSL before requirements justify it.

### Practical example
EligibleForDiscount can compose ActiveCustomer AND MinimumSpend AND NOT FraudFlagged.

### Trade-offs
Expressive reuse and testability; too much generic composition can obscure simple logic.

### Failure modes and common mistakes
Mixing JPA/SQL concerns into domain specifications; huge generic expression trees; side effects in predicates.

## Senior Questions / Exercises
1. Specification vs Strategy: predicate vs algorithm—explain.
2. How do you avoid coupling domain specs to JPA Criteria?
3. When is a named method clearer?

## Related Topics
- [Composite](../02-structural/composite.md)
- [Strategy](../03-behavioral/strategy.md)
