# Prototype

> **Wall Note / A4**
>
> **Intent:** create objects by copying a configured exemplar. **Use when:** copying is cheaper/clearer than reconstructing. **Cost:** copy semantics are tricky.


## Detailed Notes

### What and why
Prototype shifts creation from reconstruction to copying. The hard part is defining correct deep/shallow copy semantics and ownership of mutable state.

### How it works
Expose an explicit copy operation or copy constructor. Prefer domain-specific copy methods over Java Cloneable.

### When to use it
Use for expensive preconfigured objects, templates, simulations, or domain values where controlled variation from a baseline is common.

### When NOT to use it
Avoid when objects own external resources, identity must remain unique, or copying mutable graphs is ambiguous.

### Practical example
A pricing scenario can copy a baseline QuoteContext and alter assumptions for what-if calculations.

### Trade-offs
Can reduce construction cost and simplify templating; risks aliasing bugs and accidental identity duplication.

### Failure modes and common mistakes
Shallow-copying mutable collections; copying database IDs; cloning connections/locks; treating serialization as universal cloning.

## Senior Questions / Exercises
1. Why is explicit copying usually safer than Cloneable in Java?
2. Which fields of an aggregate should never be copied verbatim?
3. How would you test deep-copy isolation?

## Related Topics
- [Memento](../03-behavioral/memento.md)
- [Computer science fundamentals](https://github.com/YosrBennagra/computer-science-fundamentals)
