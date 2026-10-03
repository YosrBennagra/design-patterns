# Refactoring Decision Records

Use this one-page format before/after a meaningful pattern refactor.

## Template

### Context
What concrete code smell, failure, or change pressure exists?

### Current behavior
What works today? What must not change?

### Forces
- what varies?
- what is stable?
- transaction/lifecycle constraints?
- concurrency?
- external/provider boundaries?
- performance concerns?

### Simplest viable option
Can extraction, composition, a function, map, configuration, or framework feature solve it?

### Pattern candidate
Which pattern, if any, clarifies the design?

### Rejected alternatives
Name at least two and explain why they add cost or solve the wrong force.

### Operational consequences
- latency/network?
- transaction?
- retry/idempotency?
- thread safety?
- caching?
- observability?

### Test strategy
What unit/integration/concurrency/contract tests prove equivalence and new behavior?

### Migration plan
How can this be introduced incrementally without a risky big-bang rewrite?

### Rollback/removal trigger
What evidence would show the abstraction is not paying for itself?

---

## Worked mini-record — provider switch

### Context
Three payment providers are selected with repeated switches. Vendor DTOs leak into checkout code.

### Forces
- vendor interface mismatch;
- provider selection varies by country;
- provider timeout can be ambiguous;
- Spring owns object lifecycle.

### Simplest viable option
One `PaymentGateway` port plus one adapter per provider and a centrally injected map keyed by provider.

### Pattern candidate
Adapter + Strategy-like selection via DI.

### Rejected
- Abstract Factory: no coherent family of products yet.
- Service Locator: hides dependency.
- inheritance hierarchy for checkout: wrong variation axis.

### Operational consequences
Provider selection must remain stable for retries. Timeout recovery needs idempotency/reconciliation. Metrics and rate limits are provider-specific.

### Tests
- adapter contract tests;
- provider-selection unit tests;
- timeout/idempotency integration scenario.

### Migration
Introduce port, migrate one provider, then remove vendor types from callers provider by provider.

### Removal trigger
If only one provider remains permanently, collapse selection abstraction while preserving external adapter boundary.

## Final exercise

Write a real decision record from one of your own Spring/Angular modules. If you cannot name the concrete force and rejected simpler option, do not add the pattern yet.
