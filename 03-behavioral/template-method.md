# Template Method

> **Wall Note / A4**
>
> **Intent:** keep an invariant algorithm skeleton in a base type while subclasses customize selected steps. **Main cost:** inheritance coupling and fragile hooks.

## Detailed Notes

Template Method is most appropriate when a framework/base class truly owns lifecycle and wants carefully controlled extension points.

### Classic Java shape

```java
abstract class ImportJob {
    public final ImportResult run(Input input) {
        validate(input);
        var rows = parse(input);
        var normalized = normalize(rows);
        return persist(normalized);
    }

    protected abstract List<Row> parse(Input input);
    protected List<Row> normalize(List<Row> rows) { return rows; }
    protected abstract ImportResult persist(List<Row> rows);
}
```

The template method `run` is final so subclasses cannot bypass required steps.

### Template/callback family in Spring
`JdbcTemplate` is conceptually related but uses composition/callbacks rather than requiring your class hierarchy. The framework owns connection/resource/error boilerplate and you supply the operation.

### Template Method vs Strategy
Use Template Method when inheritance/lifecycle ownership is stable and intentional. Prefer Strategy/composition when one step/policy should vary independently or be selected at runtime.

### Hook design
Hooks should:
- have narrow contracts;
- avoid relying on partially initialized state;
- document whether calling super is required;
- avoid exposing every internal step “just in case.”

### Failure modes
- overridable method called from constructor;
- subclass can skip validation/security;
- dozens of protected hooks;
- subclasses depend on undocumented base-class internals;
- base change silently breaks subclasses.

## Senior Questions / Exercises
1. Refactor Template Method into Strategy and compare.
2. Why should invariant workflow methods often be final?
3. What makes a framework hook safe?
4. Identify a fragile-base-class problem in a real inheritance tree.

## Related Topics
- [Strategy](./strategy.md)
- [Java/Spring examples](../07-frameworks/java-spring.md)
