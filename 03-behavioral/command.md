# Command

> **Wall Note / A4**
>
> **Intent:** encapsulate a request as an object. **Signal:** queueing, logging, retries, undo, or deferred execution. **Trade-off:** more command types.


## Detailed Notes

### What and why
Command makes an action first-class, enabling transport, scheduling, auditing, retry, permission checks, and sometimes undo.

### How it works
A command contains intent/data; a handler executes it. Keep transport DTOs and domain commands distinct when semantics differ.

### When to use it
Use for job queues, task scheduling, CQRS command handlers, UI actions, and auditable workflows.

### When NOT to use it
Avoid wrapping every method call when no lifecycle or dispatch behavior is needed.

### Practical example
A CreateInvoiceCommand can be validated, authorized, queued, and handled by a specific handler.

### Trade-offs
Enables decoupled dispatch and operational features; adds ceremony and may fragment simple workflows.

### Failure modes and common mistakes
Commands with heavy domain logic; persistence entities in queued commands; retrying non-idempotent commands blindly.

## Senior Questions / Exercises
1. What makes a command safe to retry?
2. Command vs event: how do intent and ownership differ?
3. How would you design undo safely?

## Related Topics
- [System design: messaging](https://github.com/YosrBennagra/system-design)
- [Memento](./memento.md)
