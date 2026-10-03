# Concurrency & Lifecycle

> **Wall Note**
>
> Patterns describe collaboration. They do **not** automatically make that collaboration thread-safe, transaction-safe, or lifecycle-safe.

## 1. Singleton + mutable state

A Spring singleton service is shared across concurrent requests.

Bad:

```java
@Service
class PriceService {
    private Customer currentCustomer; // request-specific shared mutable state

    Money price(Customer customer, Cart cart) {
        this.currentCustomer = customer;
        return calculate(cart);
    }
}
```

Concurrent requests can overwrite state.

Prefer stateless services, immutable state, or correctly scoped/request-local state.

---

## 2. Strategy objects

Stateless strategy instances can safely be shared.

Stateful strategies require explicit ownership:
- new strategy per operation;
- immutable configuration;
- synchronization where justified;
- never hide request state in singleton bean fields.

---

## 3. Decorator / Proxy

A thread-safe delegate does not imply a thread-safe wrapper.

Example risk:
- Metrics decorator stores mutable `startTime` in an instance field.
- Singleton decorator serves many requests.
- timings corrupt each other.

Per-invocation data belongs on stack/local context.

---

## 4. Builder

Builders are normally mutable and thread-confined.

Do not register a mutable Builder as a singleton dependency and mutate it across requests.

---

## 5. Observer

Synchronous Observer can create:
- reentrancy;
- recursive event loops;
- listener-order coupling;
- long request latency.

Asynchronous Observer introduces:
- concurrent handlers;
- races;
- out-of-order completion;
- failure isolation questions.

Concurrency semantics must be part of the event contract.

---

## 6. State

Two requests can both observe `PENDING` and independently transition.

Object pattern:

```text
PENDING -> PAID
```

Persistence reality:

```text
request A reads version 5
request B reads version 5
A writes PAID version 6
B must fail/re-evaluate, not overwrite
```

Use optimistic versioning, locking, conditional update, or another invariant owner.

---

## 7. Flyweight / shared cache

Shared intrinsic objects should be immutable.

If flyweight state mutates, every logical owner may see unexpected change.

---

## 8. Repository

A repository abstraction does not define:
- transaction isolation;
- lock mode;
- persistence-context lifetime;
- detached entity semantics;
- batching behavior.

Expose important semantics in use-case-specific methods.

## Expert checklist

For every shared pattern object ask:
- Who creates it?
- Who owns it?
- How long does it live?
- Is it mutable?
- Which threads/tasks can reach it?
- Is mutation atomic?
- Is the underlying operation transactional?
- Can callbacks re-enter it?
- Can retries repeat its effects?

## Exercises

1. Find five Spring singleton classes and classify mutable state.
2. Design a state transition with optimistic locking.
3. Make a decorator chain safe under 1,000 concurrent requests.
4. Explain why `ConcurrentHashMap` alone may not make a compound workflow atomic.
5. Decide whether a strategy implementation should be singleton, request-scoped, or per-operation.
