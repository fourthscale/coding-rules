---
id: principle-design-patterns
category: archi
title: Design patterns
---

# Design patterns

Reach for a well-known pattern when it fits a real need — a variation point, an
external dependency to isolate, or complex construction. Patterns are vocabulary,
not goals: the aim is code that's easy to change and test. "Unit" = class, module,
or function, depending on the language.

## Match the pattern already in the codebase
- Before introducing a pattern, look at how the surrounding code already solves
  the same problem, and follow it. Consistency beats introducing a "better"
  pattern that now competes with the existing one.
- Only deviate deliberately, and when you do, apply it consistently — don't leave
  two patterns solving the same problem side by side.
- Severity: **major**.

## Facade — a simple entry point over a complex subsystem
- When a caller would otherwise orchestrate several units (services, adaptors,
  helpers, formatters) to get one thing done, put that orchestration behind a
  facade exposing a single, intention-revealing method. Callers (e.g. controllers)
  stay thin and don't know the moving parts.
- The facade **coordinates**; it holds no request/user state and pushes real work
  down to the units it composes — don't let it swell into a god-object that does
  the work itself.
- Severity: **minor**.

## Strategy — swap behavior instead of branching on a type
- When behavior varies by a *kind/type* and you're tempted by a growing
  `if/else`/`switch` on that type, replace it with interchangeable strategies
  (polymorphism or a lookup keyed by the type).
- This is the concrete tool for Open/Closed: adding a case becomes adding a unit,
  not editing a branch.
- Severity: **major**.

## Adapter — isolate external dependencies behind your own interface
- Wrap third-party libraries, vendor SDKs, and infrastructure clients in an
  interface **you** own. Business logic depends on your interface, never on the
  vendor's API directly.
- This keeps Dependency Inversion real, makes swapping the vendor a local change,
  and lets you test without the real dependency.
- Severity: **major**.

## Factory / Builder / Dependancy Injection — never construct collaborators inline
- Obtain services, repositories, and adaptors through the project's factory / DI
  mechanism. Never `new` a concrete collaborator, or import a concrete
  implementation, inside business logic — it hard-wires the dependency and makes
  the unit untestable. (This is Dependency Inversion made concrete.)
- Plain values / DTOs / entities may use a direct constructor. Reach for a
  **Builder** only when construction is genuinely complex (many parameters,
  conditional assembly) — a builder for a two-field object is noise.
- Severity: **major**.

## Decorator — add behavior without modifying the original
- To layer cross-cutting behavior (logging, caching, retries, validation) onto an
  existing unit, wrap it in a decorator that shares its interface — instead of
  editing the unit or subclassing it.
- Severity: **minor**.

## Observer / events — decouple producers from consumers
- When one action must trigger several independent reactions, emit an event and
  let consumers subscribe, rather than hard-wiring each call into the producer.
- Keep it for genuine one-to-many decoupling; a direct call is clearer for a
  single, known consumer.
- Severity: **minor**.

## Beware Singleton and global mutable state
- A single shared instance is acceptable **only** when obtained through the
  project's factory/DI mechanism and it holds no mutable request/user state.
- Never store per-request or per-user data in a global/singleton — it leaks
  across requests and breaks under concurrency. Use request-scoped context
  instead.
- Severity: **major**.

## Don't over-apply patterns
- No pattern for a problem you don't have. A factory with one product, a strategy
  with one implementation, or an event with one synchronous listener is
  indirection with no payoff — prefer the direct, simpler code.
- Introduce the pattern when the second case, the external boundary, or the real
  testing need actually appears.
- Severity: **info**.