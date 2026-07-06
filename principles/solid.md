---
id: principle-solid
category: archi
title: SOLID principles
---

# SOLID

Language-agnostic design rules for classes, modules, and functions. Apply them
to keep code changeable and testable — but do not over-apply them (see the last
section). "Unit" below means whatever your language uses: class, module, or
function.

## Single Responsibility (SRP)
- A unit has **one reason to change**. Keep business rules, I/O (DB, HTTP, files),
  and presentation/formatting in **separate** units — don't mix them in one place.
- Smell to fix: a class/module that both computes something *and* persists it *and*
  formats it for output. Split it.
- Don't confuse "one responsibility" with "one method" — SRP is about *reasons to
  change*, not size.
- Severity: **major**.

## Open/Closed (OCP)
- Prefer extending behavior by **adding** a new unit over **editing** a stable one,
  when a variation point already exists (strategy, polymorphism, injection).
- When you see a growing `if/switch` on a *type/kind* to pick behavior, that's the
  signal to introduce polymorphism or a lookup — not another branch.
- Do NOT invent abstraction "just in case": add the seam when the **second** case
  appears, not before.
- Severity: **minor**.

## Liskov Substitution (LSP)
- A subtype must be usable **anywhere** its base type is expected, without the
  caller knowing. Overrides must honor the base contract: no stricter inputs, no
  weaker outputs, no new exceptions the caller isn't prepared for.
- Never override a method to throw "not supported", to no-op silently, or to return
  a different shape. If a subtype can't fulfill the contract, the hierarchy is
  wrong — use composition instead.
- Severity: **major**.

## Interface Segregation (ISP)
- Depend on **narrow** interfaces holding only the methods the caller actually
  uses. Prefer several small, role-focused interfaces over one fat "does
  everything" interface.
- Smell: implementations forced to stub methods they don't need. Split the
  interface by role.
- Severity: **minor**.

## Dependency Inversion (DIP)
- High-level code (policy/business rules) must depend on **abstractions**, not on
  concrete low-level details (a specific DB client, HTTP library, vendor SDK).
- Concrete dependencies are **injected** (constructor/factory/params), never
  `new`-ed or imported directly inside the business logic. This is what makes the
  unit testable without the real infrastructure.
- Smell: importing a database/HTTP/vendor module directly inside a domain unit.
  Put it behind an interface and inject it.
- Severity: **major**.

## Don't over-apply SOLID
- These are guidelines, not quotas. Premature abstraction, one-implementation
  interfaces, and indirection with no variation point are **worse** than the
  duplication they try to avoid.
- Introduce a seam when a real second case or a real testing need appears — not
  speculatively. Simplicity beats a "correctly layered" design nobody needs yet.
- Severity: **info**.