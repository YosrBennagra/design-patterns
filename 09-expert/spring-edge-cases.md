# Spring Proxy & Transaction Edge Cases

> **Wall Note**
>
> An annotation is not the behavior. Understand **which component implements it, when the call crosses that component, and which failures trigger rollback/retry/security logic**.

## 1. Self-invocation

```java
@Service
class BillingService {
    void runBatch() {
        chargeOne(); // direct this-call
    }

    @Transactional
    void chargeOne() { /* ... */ }
}
```

With proxy-based advice, the inner call may bypass the proxy.

Preferred fix: put the actual transaction boundary around a real use case or move the transactional operation into a collaborator. Avoid self-proxy tricks unless you have a compelling reason.

---

## 2. Transaction + remote call

Bad default:

```text
BEGIN DB TX
update order
call payment provider for 8 seconds
update payment
COMMIT
```

Problems:
- DB locks/connections held while network waits;
- ambiguous provider timeout;
- retry/rollback cannot undo external charge.

Prefer explicit workflow/pending state where remote side effects are outside the local transaction.

---

## 3. Rollback rules

Do not assume every exception produces the same rollback semantics. Know your framework configuration and test the actual boundary.

A senior test proves:
- which writes commit/rollback;
- which exception path triggers what;
- what external side effect already happened.

---

## 4. Proxy ordering

A method may be affected by:
- security;
- transaction;
- retry;
- cache;
- metrics;
- tracing.

Order matters.

Example: retry outside transaction vs retry inside one transaction can produce very different results.

```text
Retry(Transaction(Method))
```

can create a fresh transaction per attempt.

```text
Transaction(Retry(Method))
```

may retry inside one transaction whose state is already invalid/rollback-only.

Do not rely on accidental advice order.

---

## 5. Lazy loading as proxy behavior

JPA lazy proxies can hide database I/O behind field/getter access.

Risks:
- N+1;
- LazyInitializationException outside persistence context;
- serialization triggers large graph loads;
- apparently local loops produce hundreds of SQL queries.

Use fetch plans/query design deliberately.

---

## 6. `@Async` and context

Async execution crosses thread boundaries. Consider:
- transaction does not magically propagate;
- security/request context may not propagate as expected;
- exceptions may no longer reach caller normally;
- thread pool saturation becomes a system resource concern.

---

## 7. Caching

Method caching via proxy/interceptor has the same call-path issue as transactions.

Also define:
- key composition;
- tenant/user scope;
- stale-data policy;
- invalidation;
- whether exceptions/null are cached;
- cache stampede behavior.

## Expert diagnostic drill

A Spring service method has:
- `@PreAuthorize`
- `@Retryable`
- `@Transactional`
- `@Cacheable`

It calls itself recursively and performs a remote provider call.

Before changing code:
1. draw the call path;
2. identify proxy crossings;
3. identify transaction lifetime;
4. identify retry ownership;
5. identify which side effects can duplicate;
6. identify cache correctness/security;
7. propose a simpler explicit design.
