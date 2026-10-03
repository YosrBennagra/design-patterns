# Flyweight

> **Wall Note / A4**
>
> **Intent:** share large amounts of duplicate immutable intrinsic state. **Rule:** profile first. **Risk:** complexity, cache growth, accidental shared mutation.

## Detailed Notes

Flyweight is primarily a memory optimization. Split object state into:
- **intrinsic** — immutable/shareable;
- **extrinsic** — varies per occurrence and is supplied by the caller.

### Example

A document renderer may have one shared `Glyph` per font/codepoint while every rendered occurrence keeps position/color outside the glyph.

```java
record GlyphKey(String font, int codePoint) {}

final class GlyphPool {
    private final ConcurrentMap<GlyphKey, Glyph> cache = new ConcurrentHashMap<>();

    Glyph get(GlyphKey key) {
        return cache.computeIfAbsent(key, Glyph::load);
    }
}
```

### Safety requirements
- flyweight state should be immutable;
- cache must be bounded or naturally bounded;
- key cardinality must be understood;
- construction cost/memory savings should be measured.

### JVM reality
Modern JVM allocation and GC are efficient. Replacing ordinary objects with flyweights without profiling can make code worse for negligible benefit.

### Related ideas
String interning, deduplicated metadata objects, prepared immutable descriptors, and canonical value caches can resemble Flyweight.

### Failure modes
- shared mutable state creates cross-request corruption;
- unbounded cache becomes the memory leak;
- synchronization cost exceeds memory benefit;
- high-cardinality keys destroy sharing.

## Senior Questions / Exercises
1. What profiler evidence would justify Flyweight?
2. Estimate memory saved if 10M nodes share 5k immutable descriptors.
3. How do you bound a flyweight pool?
4. Why is immutability central to thread safety here?

## Related Topics
- [Prototype](../01-creational/prototype.md)
- [Computer science fundamentals](https://github.com/YosrBennagra/computer-science-fundamentals)
