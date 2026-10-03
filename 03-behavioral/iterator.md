# Iterator

> **Wall Note / A4**
>
> **Intent:** traverse without exposing internal representation. **Senior concern:** lifecycle, laziness, consistency, mutation, and remote pagination semantics.

## Detailed Notes

Modern Java hides much classic Iterator ceremony behind `Iterable`, streams, spliterators, and library collections. Custom iteration is justified when traversal itself is a meaningful abstraction.

### Tree traversal example

```java
final class DepthFirstIterator implements Iterator<Node> {
    private final Deque<Node> stack = new ArrayDeque<>();

    DepthFirstIterator(Node root) { stack.push(root); }

    public boolean hasNext() { return !stack.isEmpty(); }

    public Node next() {
        var current = stack.pop();
        var children = current.children();
        for (int i = children.size() - 1; i >= 0; i--) stack.push(children.get(i));
        return current;
    }
}
```

The tree no longer exposes storage/traversal details to callers.

### Iterator vs Stream
Iterator is pull-based traversal state. Stream adds declarative operations, possible parallelism, single-use semantics, and pipeline optimization. A stream is not merely “a nicer Iterator.”

### Database/API cursor
A remote cursor is similar conceptually but has stronger operational concerns:
- result ordering must be stable;
- cursor/token should encode continuation, not client-controlled offset assumptions;
- source data may change between pages;
- cursor expiration must be defined;
- do not hold DB transactions/cursors open across user think time.

### Concurrent modification
“Fail-fast” iterators are debugging aids, not thread-safety guarantees.

### Failure modes
- iterator owns an open DB connection too long;
- repeated iteration assumed but source is one-shot;
- traversal order undocumented;
- mutation invalidates traversal state;
- offset pagination called an “iterator” despite unstable/expensive behavior.

## Senior Questions / Exercises
1. Iterator vs Stream: lifecycle and semantics?
2. Design cursor pagination over `(createdAt,id)`.
3. How does concurrent mutation affect traversal guarantees?
4. When should traversal be eager rather than lazy?
5. How would you traverse a huge tree without recursion-stack overflow?

## Related Topics
- [Composite](../02-structural/composite.md)
- [Visitor](./visitor.md)
- [System design](https://github.com/YosrBennagra/system-design)
