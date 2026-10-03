# Facade

> **Wall Note / A4**
>
> **Intent:** provide a simpler task-oriented interface over a complex subsystem. **Signal:** callers orchestrate too many collaborators. **Trade-off:** facade can become a god service.


## Detailed Notes

### What and why
Facade reduces coupling to subsystem details by offering cohesive use-case-level operations. It should simplify access, not own every responsibility.

### How it works
Expose a small API that coordinates existing components while preserving subsystem responsibilities.

### When to use it
Use at module boundaries, SDK wrappers, application services, and Angular feature services that hide several lower-level calls.

### When NOT to use it
Avoid a giant facade with unrelated use cases or duplicated domain logic.

### Practical example
An OrderCheckoutFacade can coordinate pricing, inventory reservation, payment, and confirmation while each subsystem remains responsible for its own rules.

### Trade-offs
Simplifies clients and localizes orchestration; can become an anemic catch-all.

### Failure modes and common mistakes
Hiding transaction/network boundaries; swallowing partial failures; accumulating unrelated operations.

## Senior Questions / Exercises
1. How is Facade different from an application service?
2. What signs show a facade became a god object?
3. Should a facade expose subsystem types?

## Related Topics
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
- [Mediator](../03-behavioral/mediator.md)
