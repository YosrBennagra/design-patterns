# Angular Reactive Boundaries

> **Wall Note**
>
> RxJS gives powerful Observer-like composition, but senior design is about **ownership, cancellation, error boundaries, state lifetime, and avoiding duplicated side effects**.

## 1. Cold vs shared execution

Multiple subscriptions to a cold HTTP observable can create multiple HTTP requests.

If a facade exposes data used by several consumers, decide whether execution should be:
- per subscriber;
- shared while active;
- cached/replayed;
- stored in explicit application state.

Do not add `shareReplay` mechanically; stale/error/memory semantics matter.

---

## 2. Cancellation as Strategy

For search:

```ts
query.valueChanges.pipe(
  debounceTime(250),
  distinctUntilChanged(),
  switchMap(q => api.search(q))
)
```

`switchMap` encodes a policy: **latest request wins**.

Alternatives:
- `concatMap`: preserve/order sequential work;
- `exhaustMap`: ignore new triggers while one runs;
- `mergeMap`: concurrent work.

Choosing the operator is a concurrency/ordering decision, not syntax.

---

## 3. Facade boundary

A feature facade can expose:

```ts
readonly vm$: Observable<ViewModel>;
save(command: SaveCustomer): Observable<Result>;
```

Good facade responsibilities:
- combine feature state;
- coordinate application-facing operations;
- translate feature errors/loading.

Bad facade:
- hundreds of unrelated methods;
- browser/vendor SDK details;
- all domain rules;
- global state for unrelated features.

---

## 4. Adapter boundary

Wrap:
- localStorage/sessionStorage;
- browser clipboard;
- maps/payment/vendor SDKs;
- native APIs.

This keeps platform types out of feature logic and improves testability.

---

## 5. Subscription lifecycle

Prefer template async mechanisms/signals or lifecycle-bound subscriptions. Long-lived subscriptions must have explicit teardown.

Memory leaks are only one concern; stale subscriptions can continue performing writes/network effects after a component is gone.

---

## 6. Retry

UI retry can duplicate commands.

Safe default:
- GET/read retries may be acceptable for transient failure;
- POST/payment/order commands need server-side idempotency before automatic retry.

The frontend cannot make a non-idempotent backend operation safe merely by using RxJS retry.

---

## 7. Error boundaries

Do not convert every error into `EMPTY`; that can silently hide state failure.

Decide whether error:
- is recoverable locally;
- should update feature state;
- should propagate to caller;
- should trigger navigation/auth handling;
- should be logged/observed.

## Expert exercises

1. Choose `switchMap`, `concatMap`, `exhaustMap`, or `mergeMap` for:
   - autosuggest;
   - save button;
   - bulk upload;
   - login submit.
2. Design a feature facade without turning it into a god object.
3. Diagnose duplicate HTTP calls caused by multiple subscriptions.
4. Decide where retry belongs for a checkout command.
5. Wrap a vendor map SDK behind an adapter + DI token.
