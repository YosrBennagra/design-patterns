# Anti-patterns, Smells & Overengineering

> **Wall Note / A4**
>
> **A pattern is justified by pressure, not possibility.** If the design is harder to trace than the change it protects against, remove indirection.

## Code smells that may suggest patterns

| Smell | Possible direction | Warning |
|---|---|---|
| repeated type/provider switches | Strategy / Factory / State | first centralize the conditional |
| vendor SDK everywhere | Adapter | semantics may not match one-to-one |
| subclass explosion | Bridge / Decorator / composition | inheritance itself may be unnecessary |
| long construction calls | Builder | target may lack cohesion |
| clients orchestrate many services | Facade / Service Layer | do not create a god facade |
| nested state conditionals | State | simple enum may still be best |
| duplicated traversal | Iterator / Visitor | standard streams may suffice |
| global mutable helper | remove Singleton / inject dependency | global state is often the smell |

## Common anti-patterns
- **God Object / God Service**
- **Service Locator**
- **Singleton-as-global-state**
- **Abstract-everything**
- **Factory-for-every-constructor**
- **Generic Repository everywhere**
- **Inheritance for reuse**
- **Event spaghetti**
- **Pattern cargo cult**

## Detailed Notes

### Refactoring sequence
1. Make behavior safe with tests.
2. Name the actual change pressure.
3. Simplify locally.
4. Identify the varying axis.
5. Compare plain refactoring vs pattern.
6. Introduce the smallest useful abstraction.
7. Re-check traceability, performance, failure semantics, and team comprehension.

Spring and Angular already provide DI, proxies/interceptors, observers/reactive streams, providers/factories, and lifecycle hooks. Re-implementing those mechanics often duplicates lifecycle and ordering rules.

## Senior Questions / Exercises
1. Find an unnecessary interface and justify deleting it.
2. Refactor a factory+strategy hierarchy into a map of functions; explain losses/gains.
3. Diagnose a Spring service with 14 injected dependencies.
4. Explain how open-for-extension can be abused before a real extension axis exists.

## Related Topics
- [Pattern foundations](../00-foundations/README.md)
- [Java/Spring](../07-frameworks/java-spring.md)
- [Angular](../07-frameworks/angular.md)
- [Programming principles](https://github.com/YosrBennagra/programming-principles)
