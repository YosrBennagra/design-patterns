# Facade

> **Wall Note / A4**
>
> **Intent:** expose a cohesive task-oriented interface over a complicated subsystem. **Do not:** turn “simple interface” into one god service that owns every rule.

## Detailed Notes

Facade reduces the number of subsystem concepts a caller must understand.

### Example

```java
@Service
final class CustomerOnboardingFacade {
    private final IdentityService identity;
    private final AccountService accounts;
    private final WelcomeWorkflow welcome;

    OnboardingResult onboard(OnboardCustomer cmd) {
        var identityId = identity.verify(cmd.identity());
        var account = accounts.open(identityId, cmd.plan());
        welcome.schedule(account.id());
        return new OnboardingResult(account.id());
    }
}
```

The facade coordinates a use case but does not absorb identity/account rules.

### Important boundary warning
If calls cross networks/databases, a facade must not make those failure/transaction boundaries disappear conceptually. Callers may need pending/partial outcome semantics.

### Facade vs Adapter
- Facade: simplify a subsystem.
- Adapter: translate an incompatible interface.

### Facade vs Service Layer
A Service Layer may expose the application's use cases and transaction boundaries. A Facade is a more general structural simplification concept. In business apps, one class may effectively serve both roles.

### Failure modes
- hundreds of unrelated methods;
- hidden remote calls and unexpected latency;
- swallowed partial failures;
- facade returns internal subsystem entities;
- all domain logic migrates into facade.

## Senior Questions / Exercises
1. What signals indicate a facade became a god object?
2. Should a facade return subsystem types?
3. Design facade semantics for a workflow where one downstream step may remain pending.
4. Compare an Angular feature facade with direct component use of five services.

## Related Topics
- [Service Layer](../04-enterprise/service-layer.md)
- [Mediator](../03-behavioral/mediator.md)
