# Testing Patterned Designs

> **Wall Note**
>
> A pattern is useful only if its behavior and trade-offs remain testable. Test **contracts, ordering, lifecycle, integration semantics, and failure behavior**—not just class existence.

## 1. Strategy
Contract-test all implementations against shared behavior where appropriate.

Example:
- every shipping strategy returns non-negative price;
- unsupported destination behavior is consistent;
- strategy-specific rules remain separately tested.

Do not force identical behavior where implementations intentionally differ.

---

## 2. Adapter
Use integration/contract tests at the vendor boundary:
- request mapping;
- error mapping;
- timeout/unknown behavior;
- authentication/header format;
- backward compatibility.

Mocking the adapter in every test does not prove the real SDK integration.

---

## 3. Decorator / Chain
Test order.

Given:

```text
Authorization → Cache → Database
```

prove unauthorized calls never return another user's cached data.

Test short-circuit behavior explicitly.

---

## 4. Proxy / Spring advice
Use integration tests for behavior that depends on the container:
- transaction rollback;
- method security;
- cache interception;
- AOP advice;
- lazy loading.

A pure unit test calling `new Service()` bypasses proxy mechanics.

---

## 5. Repository
Test real query semantics:
- indexes/query plans where performance matters;
- N+1 behavior;
- lock/version conflict;
- pagination correctness;
- transaction isolation assumptions.

Testcontainers or equivalent integration DB is often more valuable than mocking repository methods.

---

## 6. State
Test:
- legal transition table;
- illegal transitions;
- repeated/idempotent command behavior;
- concurrent transition conflict;
- persistence state after failure.

Property-based tests can help validate transition invariants.

---

## 7. Observer / async flows
Test:
- duplicate event;
- out-of-order event if applicable;
- listener failure;
- cancellation/unsubscription;
- event storm/backpressure assumptions.

## Test smell

If a pattern makes tests require mocking 12 collaborators for every case, ask whether the design increased coupling under a different shape.

## Expert exercises

1. Write the minimum integration test proving `@Transactional` rollback.
2. Test two concurrent order transitions using optimistic locking.
3. Contract-test two provider adapters.
4. Test decorator order without asserting implementation classes.
5. Identify which tests belong in unit, integration, contract, and concurrency suites.
