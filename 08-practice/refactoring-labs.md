# Refactoring Labs

## Lab 1 — Provider switch explosion

You inherit a Spring service:

```java
if (provider.equals("A")) { /* map DTO, call A, map response */ }
else if (provider.equals("B")) { /* different DTO and errors */ }
else if (provider.equals("C")) { /* ... */ }
```

The same switch exists in payment, refund, and status-check code.

### Tasks
1. Separate **provider-interface mismatch** from **provider-selection policy**.
2. Decide where Adapter is appropriate.
3. Decide whether Strategy is appropriate or whether DI + a map is enough.
4. Keep provider-specific errors from leaking inward.
5. Explain how idempotency and provider timeout ambiguity affect the design.

---

## Lab 2 — Transaction annotation that does not work

A method marked transactional is called from another method on the same Spring bean. Production occasionally leaves partial state.

### Tasks
1. Explain the proxy call path.
2. Move the real transaction boundary to the application use case.
3. Decide whether a separate collaborator is clearer than self-proxy tricks.
4. Write the integration test that proves rollback/atomicity.

---

## Lab 3 — Order state conditionals

An Order service contains dozens of checks:

```java
if (status == PAID) { ... }
if (status == CANCELLED) { ... }
if (status != DRAFT && status != PENDING) { ... }
```

### Tasks
1. First improve the enum-based model without State classes.
2. Define legal transitions.
3. Decide at what complexity threshold State objects become justified.
4. Add optimistic-locking/concurrency reasoning.
5. Explain how external side effects change the transition design.

---

## Lab 4 — Event spaghetti

A Spring application publishes ten application events. Listeners publish more events. Some listeners send email and call external APIs. Failures are hard to trace.

### Tasks
1. Classify events as in-process notification vs durable integration event.
2. Remove events where direct calls are clearer.
3. Move durable events behind an outbox.
4. Define listener ordering/error semantics only where truly required.
5. Add observability for event cascades.

---

## Lab 5 — Generic repository framework

Every JPA entity extends a generic repository abstraction with:
- search filters;
- generic sort;
- generic specification strings;
- include relations;
- page options.

Critical screens suffer from N+1 queries and slow OFFSET pagination.

### Tasks
1. Identify where the abstraction hides database semantics.
2. Replace generic calls with use-case-specific queries.
3. Keep locking/index/pagination decisions visible.
4. Decide where Specification is still valuable.
5. Define integration/performance tests.

---

## Lab 6 — Too many patterns

A small feature uses AbstractFactory → Factory → Strategy → Decorator → Proxy before returning a two-field DTO.

### Tasks
1. Remove every abstraction not justified by a present force.
2. Preserve only the boundary or variation that is real today.
3. Explain what future change would justify reintroducing each removed abstraction.
4. Compare cognitive complexity before/after.
