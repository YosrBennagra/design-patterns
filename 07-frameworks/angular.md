# Angular Pattern Map

> **Wall Note / A4**
>
> Angular commonly expresses patterns through **DI, providers, RxJS, interceptors, directives, components, and adapters**. Use Angular primitives before inventing parallel frameworks.

## Detailed Notes

| Angular mechanism | Pattern ideas |
|---|---|
| DI providers / injection tokens | Dependency Injection, Strategy, configurable factories |
| HttpInterceptor | Chain of Responsibility / Decorator |
| RxJS Observable | Observer / reactive composition |
| data-access service | Facade / Adapter |
| directives | Decorator-like UI behavior |
| router guards | policy chain |
| actions/reducers in state libraries | Command-like intent + state transitions |
| component tree | Composite-like UI structure |

### Practical guidance
- Keep integration translation in adapters/services rather than spreading vendor/browser APIs across components.
- Treat subscriptions as resources; prefer lifecycle-aware operators.
- Avoid a global shared service that becomes mutable application-wide state without a clear state model.
- Interceptors are good for generic transport concerns, not domain-specific business rules.

### Example decision
For incompatible payment SDKs:
1. **Adapter** each SDK to the application contract.
2. Use **Strategy/DI** to select provider policy.
3. Optionally expose a **Facade** so components do not orchestrate low-level calls.

## Senior Questions / Exercises
1. Explain Observer in RxJS terms including unsubscribe, errors, and backpressure.
2. When should an Angular interceptor not contain retries?
3. Design provider selection with injection tokens without string switches in components.
4. Identify where a Facade becomes a god service.

## Related Topics
- [Observer](../03-behavioral/observer.md)
- [Chain of Responsibility](../03-behavioral/chain-of-responsibility.md)
- [Adapter](../02-structural/adapter.md)
- [System design](https://github.com/YosrBennagra/system-design)
