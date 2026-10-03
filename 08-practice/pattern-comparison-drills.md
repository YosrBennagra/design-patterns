# Pattern Comparison Drills

Answer each in **problem/intent/trade-off** terms, not UML terms.

## Core comparisons

1. **Strategy vs State** — same delegation shape; policy selection vs lifecycle-dependent behavior.
2. **Adapter vs Facade** — interface translation vs subsystem simplification.
3. **Decorator vs Proxy** — add responsibilities vs control access.
4. **Factory Method vs Abstract Factory** — one creation extension point vs family of related products.
5. **Builder vs Factory** — stepwise complex construction vs selection/creation.
6. **Observer vs durable events** — in-process notification vs delivery/replay/idempotency requirements.
7. **Command vs event** — intent/request vs fact that already happened.
8. **Composite vs Visitor** — object tree vs operations over a stable heterogeneous tree.
9. **Template Method vs Strategy** — inheritance-controlled workflow vs composed interchangeable policy.
10. **Repository vs DAO-like table access** — domain aggregate boundary vs persistence operation abstraction.

## Scenario drills

For each scenario, pick the simplest solution and explain rejected alternatives.

### A. Tax calculation
Three tax algorithms depend on jurisdiction and are independently tested.

### B. Browser API isolation
Angular components directly use localStorage, clipboard, and window APIs.

### C. Document lifecycle
Draft → Review → Approved → Published with complex transition-specific permissions.

### D. Metrics around a stable interface
Every implementation needs timing and counters, with optional stacking.

### E. Third-party shipping SDK
The vendor names, error model, and money units differ from the domain.

### F. Large AST
Element types are stable, but type checking, rendering, and code generation keep growing.

## Quality bar

A strong answer should mention:
- why the simplest non-pattern option is insufficient;
- how the pattern changes dependency direction;
- how the framework may already supply the mechanics;
- what new complexity the pattern introduces.
