# Spring / Angular Framework Drills

## Spring

### 1. Transaction boundary
A controller calls Service A, which calls Service B, which calls a payment provider and then writes three tables.

Decide:
- where the database transaction belongs;
- whether the payment call belongs inside it;
- where idempotency/outbox belongs;
- which behavior Spring proxies provide.

### 2. Multiple implementations
Four fraud-check algorithms implement the same interface.

Compare:
- injected Map of implementations;
- explicit registry;
- Factory Method;
- Strategy;
- service locator.

### 3. Cross-cutting metrics
Compare:
- Spring AOP;
- explicit Decorator;
- servlet/filter/interceptor;
- direct instrumentation.

State when call-path visibility outweighs convenience.

### 4. Persistence abstraction
Decide whether an extra custom repository wrapper is useful when Spring Data already provides CRUD and query methods.

## Angular

### 5. HTTP concerns
Place these correctly:
- auth header;
- correlation ID;
- retry;
- domain-specific fallback;
- cache;
- validation error mapping.

Decide which belong in an interceptor and which belong in feature/application services.

### 6. RxJS lifecycle
A component subscribes to router events, form changes, and a websocket stream.

Identify:
- subscription lifetime;
- cancellation;
- backpressure/debounce;
- replay/state behavior;
- when a state store is justified.

### 7. Vendor SDK
A map SDK leaks into components.

Refactor using:
- Adapter at the integration boundary;
- optional Facade for feature operations;
- DI token/provider for test replacement.

## Final exercise

Choose one real feature from an Angular + Spring application and write a one-page decision record:
- forces;
- simplest design;
- patterns actually used;
- framework mechanics;
- patterns explicitly rejected;
- failure/concurrency/transaction concerns.
