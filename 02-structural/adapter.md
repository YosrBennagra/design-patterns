# Adapter

> **Wall Note / A4**
>
> **Intent:** translate one interface into another. **Signal:** a useful component has the wrong interface. **Trade-off:** translation boundary.


## Detailed Notes

### What and why
Adapter preserves an existing client contract while wrapping an incompatible dependency. It is especially useful at external boundaries because vendor formats stay out of core code.

### How it works
Use composition: implement the interface expected by your application and delegate to the adaptee while translating types and errors.

### When to use it
Use for legacy APIs, third-party SDKs, protocol/model translation, and Angular service wrappers around browser/vendor APIs.

### When NOT to use it
Avoid if you control both sides and can evolve the interface directly, or if the adapter becomes a dumping ground for business logic.

### Practical example
A PaymentGateway port can be implemented by StripePaymentGatewayAdapter, translating domain money/errors into SDK calls.

### Trade-offs
Decouples clients from vendors and enables testing; adds mapping code and can hide semantic mismatches.

### Failure modes and common mistakes
Pretending two models are equivalent; swallowing vendor errors; exposing vendor DTOs through the adapter.

## Senior Questions / Exercises
1. What belongs in an adapter versus a domain service?
2. How do you prevent a third-party SDK from leaking across a codebase?
3. Compare Adapter and Facade.

## Related Topics
- [Facade](./facade.md)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
