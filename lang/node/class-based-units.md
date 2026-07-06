---
id: node-class-based-units
category: archi
title: Node — class-based units
---

# Class-based units

Applies to Node/JS code following the resources/<domain>/ DDD layout. Every
domain unit — facade, service, repository, adaptor, formatter, builder, helper,
controller — is an **ES6 class**, exported as the module's single public class
(CommonJS `module.exports`), with its dependencies injected (never `new`-ed
inline).

Does NOT apply to: configuration and constants, plain data / DTO shapes, route
wiring, app entry points, ORM models and migrations, scripts and third-party glue — these
stay in their idiomatic form. Rule of thumb: if it has behavior and
collaborators, it's a unit and must be a class.

## Every unit is a class, one public class per file
- No exported-function modules for units. Each unit is a class, exported as the
  module's single public class. One public class per file; the filename mirrors
  the class name in the project's file-naming casing (camelCase or PascalCase) —
  pick one and keep it consistent across the codebase.
- Uniformity is the point: one shape everywhere makes structure predictable, every unit injectable/mockable, and the convention trivial to enforce.
  ```js
  // makePizzaFacade.js
  class MakePizzaFacade { /* ... */ }
  module.exports = MakePizzaFacade;
  ```
- Severity: **major**.

## Share contracts via abstract base classes
- Put shared structure/contract in an abstract base named `*Abstract`; concrete
  units extend it (Template Method / shared contract, not copy-paste).
  ```js
  class MakeMargheritaPizzaFacade extends MakePizzaFacadeAbstract { /* ... */ }
  ```
- Don't create an abstract base with a single implementation "just in case" — add it when a second concrete unit actually appears.
- Severity: **minor**.

## Helpers are narrow, single-purpose classes — not a dumping ground
- A helper is a small helper class (obtained via its factory like any unit), stateless — no request/user state, since it's effectively a singleton — with a single clear purpose. No business orchestration or persistence: those belong to a facade/service/repository.
- A helper class accumulating unrelated methods is a smell: split by intent.
- Severity: **minor**.

## Encapsulate by default — private first, expose only what's needed
- Default every method and property to **private**; promote to public only what a
  caller genuinely needs. A class's public surface is its contract — keep it
  minimal so internals stay free to change (Interface Segregation + encapsulation,
  applied).
- Visibility convention (prefix):
  - `#name` → **private**, language-enforced (ES2022 private fields/methods).
  - `_name` → **protected / internal**, convention only — for subclasses; never
    accessed from outside the class hierarchy. Not enforced by the runtime.
  - `name` → **public**, the intended contract.
- Start private; widen only when a real need appears — never expose "just in case".
- Severity: **minor**.