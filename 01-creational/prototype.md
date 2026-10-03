# Prototype

> **Wall Note / A4**
>
> **Intent:** create a new object from a configured exemplar. **Hard part:** identity, ownership, and deep/shallow copy semantics.

## Detailed Notes

Prototype is useful when “start from this configured baseline and vary a few things” is more natural or cheaper than reconstructing from scratch.

### Prefer explicit copy semantics

```java
record PricingScenario(
    Money basePrice,
    List<DiscountRule> rules,
    Currency currency
) {
    PricingScenario withCurrency(Currency next) {
        return new PricingScenario(basePrice, List.copyOf(rules), next);
    }
}
```

For mutable/identity-bearing aggregates, use explicit copy constructors/factories that state which fields change.

### Do not copy identity accidentally

```java
Invoice duplicateAsDraft(Invoice source) {
    return Invoice.draft(
        InvoiceId.newId(),          // new identity
        source.customerId(),
        source.lines().stream().map(InvoiceLine::copy).toList()
    );
}
```

### Deep vs shallow copy
A shallow copy is safe only when referenced state is immutable or intentionally shared. Mutable collections, buffers, child entities, and ownership relationships need explicit decisions.

### When to use
- document/template duplication;
- simulation/what-if scenarios;
- expensive immutable preconfiguration;
- game/rendering objects with controlled copied state.

### Avoid when
- object owns sockets, locks, transactions, threads, file handles;
- identity uniqueness matters;
- object graph ownership is ambiguous;
- copying would duplicate secrets or lifecycle state.

### Java note
Prefer explicit copy methods/constructors over `Cloneable`; the latter gives weak semantic guidance.

### Failure modes
- duplicate DB IDs/version fields;
- shallow-copy aliasing;
- copying cache/session/transaction state;
- copying a mutable graph that two owners then modify.

## Senior Questions / Exercises
1. What is the copy contract for an aggregate root?
2. Which fields should reset when duplicating an invoice/order?
3. How do immutable values simplify Prototype?
4. Write a test proving deep-copy isolation of nested mutable state.

## Related Topics
- [Memento](../03-behavioral/memento.md)
- [Builder](./builder.md)
