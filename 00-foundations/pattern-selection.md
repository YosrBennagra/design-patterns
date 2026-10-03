# Pattern Selection Heuristics

> **Wall Note / A4**
>
> Ask **what varies? what stays stable? who owns creation/lifecycle? where is the boundary?** Choose by intent, not class diagram.

## Detailed Notes

| Pressure / smell | First candidates | Check before using |
|---|---|---|
| conditional algorithm selection | Strategy | would a lambda/map be enough? |
| state-dependent behavior | State | is a simple enum/switch clearer? |
| incompatible third-party interface | Adapter | can you evolve your own boundary? |
| optional layered behavior | Decorator | does order matter? is middleware/AOP clearer? |
| complex subsystem orchestration | Facade | is this actually an application service? |
| family of compatible objects | Abstract Factory | can DI configuration assemble them? |
| many optional construction parameters | Builder | is the target object doing too much? |
| tree-like model | Composite | do operations apply to leaves and composites? |
| one-to-many notifications | Observer | what are delivery/lifetime semantics? |
| request pipeline | Chain of Responsibility | should control flow remain explicit? |

### Senior rule
A pattern decision is incomplete without rejected alternatives and why they were rejected.

## Senior Questions / Exercises
1. Take a repeated provider switch and propose two refactorings with different intents.
2. Identify three GoF mechanisms already supplied by Spring and explain when not to hand-roll them.
3. Show how a pattern can improve extensibility while making debugging worse.

## Related Topics
- [Pattern combinations](../05-relationships/pattern-combinations.md)
- [Anti-patterns](../06-antipatterns/README.md)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
