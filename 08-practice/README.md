# Practice & Refactoring Labs

This section converts pattern knowledge into senior engineering judgment.

The rule is simple: **never choose the pattern first**.

## Practice order

1. [Refactoring Labs](./refactoring-labs.md)
2. [Pattern Comparison Drills](./pattern-comparison-drills.md)
3. [Spring / Angular Framework Drills](./framework-drills.md)

## How to answer a lab

Use this structure:

1. **Smell / failure** — what is concretely wrong?
2. **Forces** — what varies, what must remain stable?
3. **Simplest refactor** — can a function, map, extraction, composition, or framework feature solve it?
4. **Candidate pattern** — only now name it.
5. **Rejected alternatives** — explain why they are worse here.
6. **Failure modes** — concurrency, lifecycle, transactions, error handling, hidden cost.
7. **Tests** — what behavior proves the refactoring is safe?

A senior solution can legitimately conclude **no pattern is needed**.
