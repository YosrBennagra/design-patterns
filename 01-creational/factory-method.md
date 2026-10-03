# Factory Method

> **Wall Note / A4**
>
> **Intent:** defer creation to a creator method or subtype. **Use when:** callers should depend on an abstraction, not a concrete constructor. **Cost:** more indirection.


## Detailed Notes

### What and why
Factory Method separates using an object from deciding which concrete object to create. It is valuable when creation varies independently from the workflow that consumes the object.

### How it works
A creator exposes a method returning an interface/base type. Subclasses or implementations choose the concrete product. In Java this may be a protected factory method or supplier; Spring DI often replaces hand-written factories when selection is configuration-driven.

### When to use it
Use when a framework owns a workflow but extension points choose products, construction depends on runtime type/configuration, or direct constructors spread conditional creation logic.

### When NOT to use it
Avoid when there is only one stable implementation, DI already expresses the variation cleanly, or a constructor/static factory is clearer.

### Practical example
A NotificationService can call createSender(channel) and send. In Spring, injecting a map of Channel to NotificationSender is often cleaner when the container owns creation.

### Trade-offs
Improves substitution and testability, but increases indirection and can create unnecessary subclasses.

### Failure modes and common mistakes
Creating a factory for every class; hiding lifetime/ownership rules; mixing business branching with object creation; duplicating Spring IoC.

## Senior Questions / Exercises
1. How is Factory Method different from Abstract Factory and a static factory method?
2. Refactor a switch(type) new X() hotspot without making creation harder to trace.
3. When would Spring DI be preferable to Factory Method?

## Related Topics
- [Abstract Factory](./abstract-factory.md)
- [Builder](./builder.md)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
