# Memento

> **Wall Note / A4**
>
> **Intent:** capture restorable state without exposing internals. **Works best:** bounded local undo/checkpoints. **Does not undo:** external real-world side effects.

## Detailed Notes

Memento lets an originator create an opaque snapshot and later restore from it.

### Immutable snapshot example

```java
record EditorMemento(String text, int cursor) {}

final class Editor {
    private String text;
    private int cursor;

    EditorMemento snapshot() {
        return new EditorMemento(text, cursor);
    }

    void restore(EditorMemento m) {
        this.text = m.text();
        this.cursor = m.cursor();
    }
}
```

### Memory strategy
For large objects, full snapshots can be expensive. Alternatives:
- bounded history;
- structural sharing/immutable persistent data structures;
- deltas;
- command-based undo;
- periodic checkpoints + event log.

### Memento vs event sourcing
Memento stores **state snapshots for restoration**. Event sourcing stores **domain facts as the source of truth** and reconstructs state by replay. They solve different problems.

### Distributed boundary
Restoring database state cannot unsend email, refund a card automatically, or retrieve a shipped package. Use compensation/workflow state for external effects.

### Security
Snapshots may contain secrets/PII. Treat persistence, encryption, retention, and access as seriously as the original state.

### Failure modes
- unbounded undo memory;
- shallow snapshot of mutable graph;
- snapshot schema incompatible after application upgrade;
- restoring identity/version fields incorrectly;
- using memento to fake distributed rollback.

## Senior Questions / Exercises
1. Full snapshot vs delta: when does each win?
2. How would you version persisted mementos?
3. Memento vs event sourcing?
4. Design undo for a document where images are 100 MB each.
5. Why can compensation be required even after restoring local state?

## Related Topics
- [Prototype](../01-creational/prototype.md)
- [Command](./command.md)
- [System design: transactions](https://github.com/YosrBennagra/system-design)
