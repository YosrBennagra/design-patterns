# Specification

> **Wall Note / A4**
>
> **Intent:** give a meaningful business predicate a name and make rules composable. **Use when:** rule reuse/composition/explanation matters. **Avoid:** universal generic rule engines for ordinary `if` statements.

## Detailed Notes

A Specification expresses a question about a candidate.

```java
interface Specification<T> {
    boolean isSatisfiedBy(T candidate);

    default Specification<T> and(Specification<T> other) {
        return x -> this.isSatisfiedBy(x) && other.isSatisfiedBy(x);
    }
}
```

Example:

```java
var eligible =
    activeCustomer
        .and(minimumSpend)
        .and(notFraudFlagged);
```

The names communicate business meaning better than repeated low-level Boolean expressions.

### Domain specification vs database query specification
They may look similar but have different constraints.

A domain specification can call rich domain behavior. A JPA `Specification<T>` builds SQL predicates. Forcing one abstraction to do both often leaks persistence concerns into the domain or creates rules that behave differently in memory vs SQL.

### Use when
- eligibility/policy predicates recur;
- rules are composed dynamically;
- rule names matter in ubiquitous/business language;
- explanation/audit can be attached to evaluation.

### Simpler alternatives
Use a named method when one rule has one owner:

```java
customer.isEligibleForRenewal()
```

Do not construct ten specification objects if that is clearer.

### Explainability
For complex regulated rules, return structured evaluation results instead of Boolean only:

```text
eligible=false
reasons=[ACCOUNT_SUSPENDED, LIMIT_EXCEEDED]
```

### Failure modes
- side effects inside predicate evaluation;
- one generic expression DSL replacing ordinary Java;
- persistence API types leaking into domain;
- repeated expensive remote/database calls during evaluation;
- unclear short-circuit order where cost matters.

## Senior Questions / Exercises
1. Domain Specification vs JPA Specification?
2. When is a named domain method better?
3. Design explainable eligibility rules.
4. How do you prevent repeated DB/network lookups while evaluating composed rules?
5. Specification vs Strategy: predicate vs behavior—explain.

## Related Topics
- [Composite](../02-structural/composite.md)
- [Strategy](../03-behavioral/strategy.md)
- [Repository & Unit of Work](./repository-unit-of-work.md)
