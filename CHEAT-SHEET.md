# Design Patterns — Cheat Sheet

> Choose by **intent**, not by class diagram. Hub: [software-engineer-roadmap](https://github.com/YosrBennagra/software-engineer-roadmap)

**Before any pattern ask:** What varies? What stays stable? Who owns creation/lifecycle? Where is the boundary? What simpler option did I reject?

## Creational
| Pattern | Intent (one line) | Java/Spring reality |
|---|---|---|
| Singleton | one shared instance | Spring beans are singleton *per container*, not per cluster. Keep them stateless |
| Factory Method | subclass/method decides which object to create | `static of(...)`, `@Bean` methods |
| Abstract Factory | create a *family* of compatible objects | provider families (payment, storage) wired by DI config |
| Builder | build complex/optional-parameter objects step by step | Lombok `@Builder`, `HttpRequest.newBuilder()`. Many params = maybe the object does too much |
| Prototype | create by copying an existing object | prefer copy constructors/`with...` over `clone()`; watch shallow vs deep copy |

## Structural
| Pattern | Intent | Example |
|---|---|---|
| Adapter | convert *their* interface to *yours* | wrap vendor SDK behind your port |
| Facade | one simple door to a complex subsystem | application service, Angular feature facade |
| Decorator | add behaviour, **same interface**, stackable | `BufferedInputStream`, HTTP interceptors. Order matters |
| Proxy | stand-in that **controls access** (lazy, remote, security, tx) | Spring AOP, `@Transactional`, JPA lazy proxies |
| Composite | treat a tree of parts and wholes uniformly | rule trees, UI trees, menus |
| Bridge | split abstraction from implementation so both vary | notification × channel |
| Flyweight | share immutable intrinsic state | `Integer.valueOf` cache, string interning |

## Behavioural
| Pattern | Intent | Example |
|---|---|---|
| Strategy | swap algorithm at runtime | `Map<Type, PricingStrategy>` injected by Spring, `Comparator` |
| State | behaviour changes with internal state | order lifecycle; an enum + switch may be enough |
| Template Method | fixed skeleton, steps overridden | `JdbcTemplate`, `RestTemplate` (callbacks) |
| Observer | notify subscribers of changes | Spring events, RxJS. In-memory events are not durable |
| Chain of Responsibility | pass a request along ordered handlers | Spring Security filter chain, servlet filters |
| Command | request as an object (queue, retry, undo) | job/command objects, CQRS commands |
| Mediator | central object coordinates peers | complex UI form coordination |
| Iterator | traverse without exposing structure | `Iterator`, streams, DB cursors |
| Memento | snapshot/restore state without breaking encapsulation | undo, drafts |
| Visitor | add operations over a stable type hierarchy | AST walks. Modern Java: sealed types + pattern-matching `switch` |

## Enterprise / application
| Pattern | One line |
|---|---|
| Service Layer | use-case boundary: transaction, authorization, orchestration |
| Repository | collection-like access to aggregates; hides persistence |
| Unit of Work | track changes, commit once (JPA persistence context) |
| DTO + Mapper | never expose JPA entities over HTTP; map at the boundary |
| Specification | composable business predicates (`and/or/not`) |
| Dependency Injection | constructor injection, composition root, no service locator |

## Look-alikes (classic interview)
| A vs B | Difference |
|---|---|
| Adapter vs Facade | Adapter converts one interface; Facade simplifies many |
| Decorator vs Proxy | Decorator **adds** behaviour; Proxy **controls access** (same shape) |
| Strategy vs State | Strategy chosen by client; State switches itself |
| Strategy vs Template Method | composition (inject) vs inheritance (override) |
| Bridge vs Adapter | Bridge designed up front; Adapter retrofitted |
| Command vs Event | command = "do this" (one handler); event = "this happened" (0..n listeners) |
| Mediator vs Observer | central coordinator vs broadcast to subscribers |

## Spring proxy gotchas (very common senior question)
- **Self-invocation:** `this.chargeOne()` skips the proxy, so `@Transactional`, `@Async`, `@Cacheable`, `@Retryable` and `@PreAuthorize` silently don't apply. Fix: move the method to another bean / put the boundary on the real use case.
- **Private methods are never proxied.** In Spring 6, class-based (CGLIB) proxies also advise protected/package-private methods. Make transactional methods public to be safe.
- **Default rollback:** `RuntimeException` + `Error` roll back; **checked exceptions commit** unless `rollbackFor`.
- **Advice order matters:** `Retry(Tx(method))` = new tx per attempt; `Tx(Retry(method))` retries inside a tx that may already be rollback-only.
- **No remote calls inside a DB transaction.** That holds locks and connections, and you can't roll back the external side effect.
- **Lazy proxies** hide SQL → N+1, `LazyInitializationException`.
- `@Async`: transaction and security context don't propagate; exceptions don't reach the caller.

## Anti-patterns
- Pattern-itis: Factory + Strategy + Interface for a single `if`.
- God object / "Manager" / "Helper".
- Generic repository over everything; anemic domain with all logic in services.
- Singleton with mutable state (thread-safety + hidden global).
- Event spaghetti: nobody can trace the flow.

## When to use what
| Pressure | First candidate | Check first |
|---|---|---|
| `switch` on type that keeps growing | Strategy | would a lambda/`Map` be enough? |
| behaviour depends on lifecycle state | State | is an enum + switch clearer? |
| vendor/legacy interface | Adapter | can you change your own boundary? |
| optional cross-cutting layer | Decorator / AOP | does order matter? |
| messy subsystem | Facade | isn't it just an application service? |
| many optional ctor params | Builder | is the object too big? |
| ordered request pipeline | Chain | should control flow stay explicit? |
| one-to-many notification | Observer | delivery + lifetime semantics? durable needed? |

**Senior rule:** a pattern decision is incomplete without the rejected alternatives and the debugging cost it adds.
