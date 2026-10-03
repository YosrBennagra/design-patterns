# Mediator

> **Wall Note / A4**
>
> **Intent:** centralize interaction rules among collaborating objects. **Signal:** many-to-many coordination coupling. **Trade-off:** mediator can become too powerful.


## Detailed Notes

### What and why
Mediator reduces direct coupling among peers by moving coordination into a dedicated component.

### How it works
Participants communicate through the mediator, which routes or coordinates. Keep domain responsibilities in participants; mediator owns interaction policy.

### When to use it
Use for dialog/component coordination, workflow coordinators, and in-process message mediation.

### When NOT to use it
Avoid turning all application behavior into one central mediator or hiding poor module boundaries.

### Practical example
A checkout mediator can coordinate mutually dependent UI sections; a workflow coordinator can mediate backend services.

### Trade-offs
Reduces peer coupling; centralization can become a god object.

### Failure modes and common mistakes
Business logic accumulating in mediator; participants still depending directly on peers; large dispatch switches.

## Senior Questions / Exercises
1. Mediator vs Facade: who initiates interactions?
2. How do you prevent a mediator from becoming a god object?
3. When is an event bus preferable?

## Related Topics
- [Facade](../02-structural/facade.md)
- [Observer](./observer.md)
