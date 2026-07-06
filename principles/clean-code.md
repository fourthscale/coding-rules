---
id: principle-clean-code
category: conventions
title: Clean code
---

# Clean code

Craft guidelines that apply to any function, method, or module, in any language.
The goal is code that's easy to read and change — not aesthetics. "Unit" below
means function/method unless stated otherwise.

## Intention-revealing names
- Name things for what they mean, not how they're built. A reader should grasp a
  variable/function's purpose without chasing its definition.
- Consistent casing per role; `is`/`has`/`can` for booleans; no cryptic
  abbreviations, no type-encoding prefixes. Longer-but-clear beats short-but-vague.
- Severity: **minor**.

## Small functions that do one thing
- A unit does one thing at a single level of abstraction. If you need "and" to
  describe it, or it mixes high-level policy with low-level detail, split it.
- Length is a symptom, not the rule: the real test is one responsibility, one
  reason to read it top to bottom.
- Severity: **minor**.

## Prefer flat control flow
- Use guard clauses and early returns to handle edge cases up front; keep the
  happy path un-indented. Avoid deep nesting and `else` after a `return`.
- Deeply nested conditionals are a smell — extract or invert instead.
- Severity: **minor**.

## Few arguments, no flag parameters
- Keep parameter lists short. Many parameters usually means a missing object or a
  unit doing too much.
- Don't pass a boolean that switches behavior — it means the unit does two things.
  Split it into two intention-revealing units.
  render(true) // ❌ what is true?
  renderDraft() renderFinal() // ✅
- Avoid output parameters (mutating an argument to return a result); return a
value instead.
- Severity: **minor**.

## Command–Query Separation
- A unit either **does** something (command, changes state, returns nothing) or
**answers** something (query, returns a value, no side effects) — never both.
- A getter that mutates, or a query with a hidden write, surprises every caller.
- Severity: **minor**.

## No magic values
- Replace unexplained literals with named constants that carry the meaning.
  if (age > 17) // ❌ why 17?
  if (age >= LEGAL_AGE) // ✅
- Severity: **minor**.

## Immutability by default
- Prefer immutable data. Don't mutate inputs, shared state, or objects you don't
own — return new values instead. Declare bindings as constant unless they must
reassign.
- Severity: **minor**.

## DRY — with the rule of three
- Remove genuine duplication of knowledge (a rule that must change in lockstep).
- But don't abstract on the first repetition: two similar-looking pieces that can
evolve independently are not duplication. Extract on the third real occurrence.
Premature abstraction is worse than a little duplication.
- Severity: **minor**.

## Comments say why, not what — and delete the rest
- Comment intent, trade-offs, and non-obvious constraints. Don't narrate what the
code already says.
- Delete dead code and commented-out blocks — version control remembers them.
Keep comments truthful; a stale comment is worse than none.
- Severity: **minor**.

## Match the surrounding code
- Follow the conventions, patterns, and structure already in the file/module over
personal preference. Consistency lets readers predict; one-off "better" styles
fragment the codebase.
- Severity: **minor**.

## Formatting is the formatter's job; readability beats cleverness
- Don't hand-argue whitespace, quotes, or line length — delegate to an
autoformatter and move on. These rules are about meaning, not layout.
- Prefer the clear solution over the clever one. Code is read far more than
written; optimize for the next reader, not for terseness.
- Severity: **info**.