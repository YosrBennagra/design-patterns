# Expert Production Reasoning

This section is where pattern knowledge stops being vocabulary and becomes production engineering judgment.

## Study order

1. [Combined Pattern Case Studies](./combined-pattern-case-studies.md)
2. [Concurrency & Lifecycle](./concurrency-lifecycle.md)
3. [Spring Proxy / Transaction Edge Cases](./spring-edge-cases.md)
4. [Angular Reactive Boundaries](./angular-reactive-boundaries.md)
5. [Testing Patterned Designs](./testing-patterned-design.md)
6. [Refactoring Decision Records](./refactoring-decision-records.md)

## Expert answer frame

For a real design/refactoring, answer:

1. What concrete failure/change pressure exists?
2. What is the simplest design that solves it?
3. Which pattern mechanics are actually needed?
4. Which mechanics are already supplied by the framework?
5. Where are object lifetime and mutable state owned?
6. What happens under concurrent calls?
7. Where is the transaction boundary?
8. What happens after timeout/retry/duplicate delivery?
9. Which behavior is synchronous vs asynchronous?
10. What telemetry proves it works?
11. How will you migrate from the current design?
12. What would make you remove the pattern later?

A pattern is not “expert” because the class diagram is complicated. It is expert when its **operational consequences are understood**.

[← Repository home](../README.md)
