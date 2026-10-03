# Observer

> **Wall Note / A4**
>
> **Intent:** notify dependents when state/events change. **Signal:** one-to-many reaction without tight coupling. **Trade-off:** temporal coupling.


## Detailed Notes

### What and why
Observer decouples publishers from subscribers but introduces callback/reactive reasoning. Delivery semantics matter more than the class diagram.

### How it works
A subject manages subscriptions or publishes via an event mechanism. Define ordering, errors, backpressure, lifetime, and sync vs async behavior.

### When to use it
Use for UI events, in-process domain notifications, reactive streams, and state subscriptions.

### When NOT to use it
Avoid for critical cross-service guarantees unless durable messaging handles delivery semantics.

### Practical example
Angular RxJS Observables are a practical observer/reactive model; Spring application events are in-process, not durable messaging.

### Trade-offs
Loose coupling and extensibility; can cause cascades, leaks, reentrancy, and hard-to-follow control flow.

### Failure modes and common mistakes
Never unsubscribing; assuming order; doing long work synchronously; confusing in-memory events with durable integration events.

## Senior Questions / Exercises
1. What delivery guarantees does in-process Observer not provide?
2. How do backpressure and unsubscribe change the classic pattern?
3. When should an Observer event become a durable integration event?

## Related Topics
- [Angular examples](../07-frameworks/angular.md)
- [System design: queues/streams](https://github.com/YosrBennagra/system-design)
