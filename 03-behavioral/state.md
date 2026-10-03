# State

> **Wall Note / A4**
>
> **Intent:** let behavior change with object state by delegating to state objects. **Signal:** state-dependent conditionals are spreading. **Trade-off:** more state types.


## Detailed Notes

### What and why
State turns mode-dependent behavior into explicit state objects. It is useful when transitions and valid operations matter.

### How it works
The context delegates state-sensitive behavior to the current state. Transitions are explicit and preserve invariants.

### When to use it
Use for workflows, protocols, order lifecycles, parsers, and UI modes.

### When NOT to use it
Avoid for a tiny enum with one simple switch that is clearer than a class hierarchy.

### Practical example
An Order can delegate pay/cancel/ship rules to state objects, though a state-machine library may be better for a large workflow.

### Trade-offs
Localizes state behavior and removes conditional sprawl; can create many classes and obscure the overall graph.

### Failure modes and common mistakes
Illegal transitions; persistence desynchronization; unrelated data in state objects; overengineering simple enums.

## Senior Questions / Exercises
1. State vs Strategy: what changes and who chooses it?
2. When should you use a state-machine library?
3. How would you persist workflow state safely?

## Related Topics
- [Strategy](./strategy.md)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
