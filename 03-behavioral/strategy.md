# Strategy

> **Wall Note / A4**
>
> **Intent:** encapsulate interchangeable algorithms/policies. **Signal:** conditional algorithm selection. **Trade-off:** selection logic still exists somewhere.


## Detailed Notes

### What and why
Strategy separates a stable workflow from a varying policy. It is one of the most useful everyday patterns because composition and DI make it lightweight.

### How it works
Define a small policy interface and inject/select an implementation. Keep contracts narrow.

### When to use it
Use for pricing, routing, validation, sorting, retry policy, serialization, and provider-specific algorithms.

### When NOT to use it
Avoid when variants are trivial lambdas or when variation is lifecycle state rather than policy.

### Practical example
Spring can inject a list/map of PricingStrategy beans; Angular can use injection tokens/providers.

### Trade-offs
Easy substitution/testing and open/closed extension; too many tiny strategies can fragment logic.

### Failure modes and common mistakes
Huge strategy interfaces; repeated string switches for selection; leaking selection details to every caller.

## Senior Questions / Exercises
1. Strategy vs State: same shape, different intent—explain.
2. How would you register strategies in Spring without a giant switch?
3. When is a lambda enough?

## Related Topics
- [Factory Method](../01-creational/factory-method.md)
- [State](./state.md)
