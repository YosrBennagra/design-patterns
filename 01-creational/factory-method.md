# Factory Method

> **Wall Note / A4**
>
> **Intent:** move concrete creation behind an extension point. **Signal:** object creation varies independently from the workflow that uses it. **Cost:** indirection and lifecycle ownership must remain visible.

## Detailed Notes

Factory Method separates **using a product** from **choosing how that product is created**. The useful design pressure is not “constructors are bad”; it is that creation policy changes independently.

### Java example

```java
interface Exporter { byte[] export(Report report); }

abstract class ExportJob {
    public final byte[] run(Report report) {
        validate(report);
        return createExporter().export(report);
    }

    protected abstract Exporter createExporter();

    private void validate(Report report) { /* shared workflow */ }
}

final class PdfExportJob extends ExportJob {
    protected Exporter createExporter() { return new PdfExporter(); }
}
```

The workflow is stable; subclasses vary product creation.

### Spring reality
If the only reason for a factory is “choose one bean by configuration,” Spring DI is often simpler:

```java
@Service
final class ExportService {
    ExportService(Map<String, Exporter> exporters) { /* ... */ }
}
```

Use a hand-written factory when creation itself has meaningful runtime rules: scoped lifetime, dynamic credentials, expensive setup, or creation from external metadata.

### Factory Method vs nearby options

| Need | Prefer |
|---|---|
| one clear constructor | constructor |
| named construction/validation | static factory |
| runtime product selection | factory/DI registry |
| family of related products | Abstract Factory |
| stepwise complex construction | Builder |
| framework controls workflow, subclass chooses product | Factory Method |

### Failure modes
- Factory per class with no variation.
- Factory method hides expensive/network creation.
- Factory owns business decisions unrelated to construction.
- Rebuilding container functionality manually.
- Scope/lifecycle leaks: returning a shared mutable product where caller expects a fresh one.

## Senior Questions / Exercises
1. Static factory vs Factory Method: why are they different patterns?
2. When is Spring DI enough, and when is runtime creation a real factory concern?
3. Refactor a `switch(type) new X()` hotspot while keeping creation traceable.
4. How would you test lifecycle semantics of a factory returning pooled vs fresh objects?

## Related Topics
- [Abstract Factory](./abstract-factory.md)
- [Builder](./builder.md)
- [Dependency Injection](../04-enterprise/dependency-injection.md)
