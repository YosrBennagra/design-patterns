# Singleton

> **Wall Note / A4**
>
> **Intent:** ensure one logical instance and controlled access. **Use rarely:** uniqueness must be a real invariant. **Cost:** global state/coupling.


## Detailed Notes

### What and why
Singleton is often overused. The key question is whether the system truly requires one logical instance, not whether one object is convenient to reach globally.

### How it works
In plain Java, enum singletons avoid many classic implementation hazards. In Spring, default bean scope is singleton per application context, not one instance across a cluster.

### When to use it
Use for stateless shared services managed by a container or genuinely unique process-level coordinators. Prefer DI so dependencies remain explicit.

### When NOT to use it
Avoid mutable global state, per-request/user data, test fixtures, or anything assumed to be singleton across multiple service instances.

### Practical example
A Spring Service can be singleton-scoped and stateless. Cluster-wide uniqueness needs distributed coordination, not Singleton.

### Trade-offs
Cheap sharing and simple lifecycle; can hide dependencies, create contention, and leak state across tests.

### Failure modes and common mistakes
Confusing Spring scope with distributed uniqueness; mutable fields in singleton beans; getInstance everywhere instead of DI.

## Senior Questions / Exercises
1. What does Spring singleton actually guarantee?
2. How would you implement cluster-wide leader uniqueness?
3. Why does mutable singleton state create concurrency bugs?

## Related Topics
- [Dependency Injection](../04-enterprise/dependency-injection.md)
- [System design](https://github.com/YosrBennagra/system-design)
