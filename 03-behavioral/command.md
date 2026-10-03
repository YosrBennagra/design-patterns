# Command

> **Wall Note / A4**
>
> **Intent:** make a request/intent a first-class value. **Useful when:** dispatch, queueing, audit, scheduling, retries, permissions, or undo have their own lifecycle.

## Detailed Notes

A command says **“please perform this action.”** It is not the same as an event, which says **“this fact already happened.”**

### Java application command

```java
public record PlaceOrder(
    UUID requestId,
    CustomerId customerId,
    List<LineInput> lines
) {}

@Component
final class PlaceOrderHandler {
    @Transactional
    OrderId handle(PlaceOrder command) {
        // load, execute domain behavior, persist, append outbox
    }
}
```

The command contains intent/data; the handler owns application orchestration. Domain behavior should not be reduced to data-only commands plus giant handlers.

### Queued commands
For a queued command:
- include stable IDs, not attached JPA entities;
- version/schema the message;
- define idempotency;
- define expiration/deadline;
- define authorization at acceptance/execution as required;
- record outcome/retry state.

### Command vs event

| Command | Event |
|---|---|
| imperative intent | past-tense fact |
| usually one logical owner | zero/many interested consumers |
| may be rejected | should not be “rejected” as if it never happened |
| retry safety must be designed | duplicate delivery must be designed |

### Undo
Undo is easy only for reversible local state. For external effects, use explicit compensating commands rather than pretending the original action disappeared.

### Failure modes
- every method wrapped in a command with no lifecycle benefit;
- command contains persistence entity graph;
- commands execute non-idempotent effects on blind retry;
- one generic `ExecuteCommand(type, payload)`;
- handler contains all domain rules.

## Senior Questions / Exercises
1. Make a payment command retry-safe after timeout.
2. Command vs event for `OrderPlaced`—which is which and why?
3. When is direct method invocation simpler?
4. Design command expiration for “send this notification within 5 minutes.”
5. How would authorization work for delayed commands?

## Related Topics
- [Observer](./observer.md)
- [Memento](./memento.md)
- [System design: queues/streams](https://github.com/YosrBennagra/system-design)
