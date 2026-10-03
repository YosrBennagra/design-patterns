# Memento

> **Wall Note / A4**
>
> **Intent:** capture and restore state without exposing internals. **Signal:** undo/checkpoint requirements. **Trade-off:** snapshot cost.


## Detailed Notes

### What and why
Memento encapsulates a state snapshot so an originator can later restore it while preserving encapsulation.

### How it works
The originator creates/restores opaque mementos; a caretaker stores them without interpreting internals.

### When to use it
Use for editors, simulations, workflow checkpoints, and bounded undo histories.

### When NOT to use it
Avoid large unbounded snapshots, sensitive state snapshots, or distributed workflows where compensation is more appropriate.

### Practical example
An editor stores immutable document-state mementos for undo/redo.

### Trade-offs
Simple rollback model; snapshots can be expensive and cannot undo external side effects.

### Failure modes and common mistakes
Snapshotting credentials; retaining huge graphs; assuming emails/payments disappear on restore.

## Senior Questions / Exercises
1. Memento vs event sourcing: what is stored and why?
2. How would you bound undo memory?
3. When must compensation replace rollback?

## Related Topics
- [Prototype](../01-creational/prototype.md)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
