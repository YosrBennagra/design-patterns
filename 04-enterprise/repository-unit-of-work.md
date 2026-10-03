# Repository & Unit of Work

> **Wall Note / A4**
>
> **Repository:** collection-like boundary for aggregate persistence. **Unit of Work:** coordinates changes as one transaction. **Do not:** create generic CRUD wrappers that hide query reality.


## Detailed Notes

### What and why
Repository protects domain/application code from persistence details when that separation has value. Unit of Work coordinates changes within one persistence transaction.

### How it works
Model repository operations around aggregates/use cases, not tables. In Spring Data/JPA, repositories and the persistence context already implement much of this behavior.

### When to use it
Use when domain logic benefits from a persistence boundary and change isolation/testability justify it.

### When NOT to use it
Avoid redundant wrappers over Spring Data with identical CRUD methods or abstractions that pretend expensive queries are cheap.

### Practical example
OrderRepository.findForUpdate(id) can express domain needs. A transactional application service defines the unit-of-work boundary.

### Trade-offs
Improves boundary clarity; query, locking, batching, and transaction semantics can still leak because they matter.

### Failure modes and common mistakes
N+1 hidden behind repositories; generic base repository everywhere; returning persistence entities across APIs; transactions too wide.

## Senior Questions / Exercises
1. When is Spring Data already the repository?
2. How can a repository hide harmful performance costs?
3. Where should transaction boundaries live?

## Related Topics
- [System design: transactions](https://github.com/YosrBennagra/system-design)
- [Software architecture](https://github.com/YosrBennagra/software-architecture)
