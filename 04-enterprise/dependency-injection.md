# Dependency Injection

> **Wall Note / A4**
>
> **Intent:** receive collaborators instead of locating/constructing them. **Why:** explicit dependencies + replaceable implementations + managed lifecycle. **Risk:** container magic/service locator.


## Detailed Notes

### What and why
DI inverts control over object assembly. It separates behavior from wiring and is foundational to Spring and Angular.

### How it works
Prefer constructor injection. Keep the composition root/container responsible for wiring. Use qualifiers only for real semantic variation.

### When to use it
Use for services with collaborators, infrastructure ports, policies, and framework-managed components.

### When NOT to use it
Do not inject trivial value objects everywhere, hide dependencies behind static context access, or call the container from domain code.

### Practical example
Spring constructor injection makes dependencies final and testable. Angular providers and injection tokens bind abstractions/configuration to implementations.

### Trade-offs
Improves substitution and lifecycle management; large graphs and configuration magic can become hard to reason about.

### Failure modes and common mistakes
Field injection, circular dependencies, service locator usage, too many dependencies indicating poor cohesion, singleton beans holding request state.

## Senior Questions / Exercises
1. Why is constructor injection preferable to field injection?
2. What does a circular dependency tell you about design?
3. How do Spring scopes change lifecycle/concurrency assumptions?

## Related Topics
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
- [Singleton](../01-creational/singleton.md)
