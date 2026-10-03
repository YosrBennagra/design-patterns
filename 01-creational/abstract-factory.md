# Abstract Factory

> **Wall Note / A4**
>
> **Intent:** create families of related objects without naming concrete classes. **Use when:** products must vary together. **Cost:** adding a new product role is expensive.


## Detailed Notes

### What and why
Abstract Factory centralizes creation of a compatible family of products. The key value is family consistency, not merely avoiding constructors.

### How it works
Define a factory interface with methods for each product role. Each concrete factory returns a coherent family. Clients depend only on product and factory abstractions.

### When to use it
Use for pluggable platform families, environment-specific integration clients, or test/production object families that must remain compatible.

### When NOT to use it
Avoid for a single product, unrelated products, or when DI profiles/configuration express the same choice more transparently.

### Practical example
A payment integration factory can supply PaymentClient, RefundClient, and WebhookVerifier for a provider while preserving family compatibility.

### Trade-offs
Strong family consistency and easy family replacement; weaker extensibility when adding a brand-new product role because all factories must change.

### Failure modes and common mistakes
Using it as a service locator; allowing downcasts; mixing unrelated products into one huge factory.

## Senior Questions / Exercises
1. Why is Abstract Factory strong for adding families but weak for adding product roles?
2. How would you model provider-specific clients in Spring without a service locator?
3. Compare Abstract Factory with configuration + DI profiles.

## Related Topics
- [Factory Method](./factory-method.md)
- [Dependency Injection](../04-enterprise/dependency-injection.md)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
