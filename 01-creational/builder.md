# Builder

> **Wall Note / A4**
>
> **Intent:** make complex construction readable and validate the final object. **Best use:** immutable objects with many optional/conditional values. **Smell:** Builder used to hide an object with too many responsibilities.

## Detailed Notes

Builder is valuable when construction has enough structure that a long constructor or many overloaded constructors obscure meaning.

### Java example

```java
var query = ReportQuery.builder()
    .tenantId(tenantId)
    .from(from)
    .to(to)
    .format(PDF)
    .includeArchived(false)
    .sortBy(CREATED_AT)
    .build();
```

The `build()` step should protect invariants:

```java
public ReportQuery build() {
    if (tenantId == null) throw new IllegalStateException("tenantId required");
    if (from != null && to != null && from.isAfter(to)) {
        throw new IllegalStateException("invalid date range");
    }
    return new ReportQuery(this);
}
```

### Builder vs alternatives

| Situation | Prefer |
|---|---|
| 2–4 obvious required values | constructor/record |
| named creation rule | static factory |
| many optional settings | Builder |
| many mandatory ordered stages | possibly staged builder |
| target keeps accumulating unrelated fields | redesign target, not Builder |

### JPA warning
Generated builders on persistence entities can bypass invariants, create partially initialized aggregates, and encourage direct construction where domain methods should own state changes.

### Concurrency
Builders are normally mutable construction helpers. Treat them as short-lived and thread-confined.

### Failure modes
- `build()` accepts invalid combinations.
- Builder duplicates all setters and adds no value.
- Workflow/network calls inside builder methods.
- Reusing one mutable builder across requests/threads.
- Lombok-generated builder bypasses important constructor/domain logic.

## Senior Questions / Exercises
1. When is a static factory clearer than Builder?
2. Design a builder for mutually exclusive `csvOptions` vs `pdfOptions`.
3. Why can builders be dangerous on JPA aggregates?
4. What invariant belongs in `build()` versus a domain service?

## Related Topics
- [Factory Method](./factory-method.md)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
