---
name: code-design
description: Use when designing, implementing, modifying, or refactoring code, especially when deciding module boundaries, abstractions, reuse, data flow, ownership, coupling, cohesion, SOLID/YAGNI/DRY/KISS tradeoffs, testability, or change size before editing code
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# Code Design

## Overview

Design code changes that fit the existing system, stay small, and remain easy to test, review, and change. The core rule is: understand the current shape before adding a new one.

## First Pass

Before editing code, answer briefly:

1. What behavior, capability, or maintainability problem is actually changing?
2. Where does the existing system already solve something similar?
3. Which module should own the change?
4. What dependencies or boundaries will the change cross?
5. Is a new abstraction needed, and what variation does it isolate?
6. What is the smallest coherent, verifiable change?

For tiny local edits, answer mentally. For risky, cross-cutting, or ambiguous changes, state the design before editing.

## Core Principles

| Principle | Apply It As |
| --- | --- |
| YAGNI | Do not build for hypothetical future requirements. Add extension points only when the current change needs them or the architecture already requires them. |
| KISS | Prefer the simplest design that clearly satisfies the requirement. Avoid cleverness, hidden control flow, and unnecessary layers. |
| DRY | Remove meaningful duplication of knowledge or behavior, not every repeated line. Repetition can be cheaper than a bad abstraction. |
| SOLID | Use as design pressure, not ceremony: single responsibility, open/closed where variation exists, substitutable contracts, focused interfaces, and dependencies pointing toward abstractions when it reduces coupling. |
| SoC | Keep UI, domain rules, persistence, infrastructure, and integration concerns separate unless the project intentionally combines them. |
| High cohesion | Keep related behavior and data together near their owner. |
| Low coupling | Minimize knowledge between modules; expose stable interfaces and hide internal details. |
| Encapsulation | Make illegal states hard to represent and implementation details easy to change. |
| Composition over inheritance | Prefer small collaborating parts unless inheritance is already the local idiom or domain fit. |
| Principle of least surprise | Match existing naming, architecture, error handling, and dependency direction. |

## Design Checklist

Use this checklist before implementing or refactoring:

- Fit existing patterns before introducing a new pattern.
- Put code close to the feature or owner first; move outward only for real reuse.
- Keep one reason to change per module where practical.
- Keep APIs narrow: expose what callers need, not internals.
- Make data flow explicit enough to trace.
- Keep error, nullability, and fallback behavior visible at boundaries.
- Avoid global state unless the project already uses it for that concern.
- Design for testability through clear boundaries, not test-only architecture.
- Prefer small, reviewable changes over broad rewrites.

## Abstraction Rules

Add an abstraction only when it pays rent:

- It removes meaningful duplication in behavior or knowledge.
- It isolates a real variation point that already exists.
- It makes callers simpler, not just the implementation fancier.
- It matches an established local pattern.
- It lets risky details change behind a stable boundary.

Do not add an abstraction when:

- There is only one caller and no current variation.
- The name would be generic, such as `Manager`, `Handler`, `Service`, or `Helper`, without a precise responsibility.
- Callers become harder to read.
- It mainly anticipates future requirements.
- It mixes unrelated concerns to look reusable.

## Boundary Rules

Keep responsibilities clear:

- UI/presentation should not own durable business rules or persistence details.
- Domain/application logic should not depend on view widgets, CLI formatting, or transport-specific payloads unless that is the explicit architecture.
- Data access should hide storage/query details behind a stable interface where the project has such layers.
- Integration code should translate external formats at the edge.
- Shared utilities should be boring, small, and truly shared.

When boundaries are unclear, inspect nearby code and ask before creating a parallel architecture.

## Refactoring Rules

Refactoring changes structure without changing behavior.

- Separate refactoring from feature behavior when practical.
- Preserve behavior with tests, snapshots, typechecks, or targeted manual verification.
- Move in small reversible steps.
- Keep public contracts stable unless the task is explicitly to change them.
- Do not rename, move, and change behavior in the same step unless the change is tiny and obvious.

If a refactor reveals a needed behavior change, stop and name it separately.

## Change Size

Prefer the smallest coherent change that can be reviewed and reverted independently. Split work when a change combines unrelated goals, different verification strategies, or multiple architectural decisions.

A design is too large when the summary needs "and also", when tests cannot isolate the behavior, or when rollback would remove unrelated value.

## Testing Implications

Design and tests shape each other:

- Put logic where it can be tested without heavy UI, network, clock, filesystem, or database setup when possible.
- Prefer testing behavior and contracts over private implementation details.
- Add seams for real external dependencies, not for every function.
- Use existing project testing style before inventing a new harness.
- If code is hard to test, first ask whether responsibilities are mixed or dependencies point the wrong way.

## Red Flags

Stop and redesign or ask when:

- A single-use requirement creates a generic framework.
- The proposed design uses vague names like `Manager`, `Service`, `Handler`, `Util`, or `Common` without a crisp contract.
- Business rules are buried in UI, scripts, database queries, or transport adapters.
- A change touches many layers without explaining data flow and ownership.
- You are adding configuration, plugins, strategies, or factories for imagined future needs.
- Refactoring and behavior changes are tangled.
- Reuse makes call sites harder to understand.
- You are replacing an established local pattern without checking why it exists.
- The design needs a long explanation to justify a small change.

## Common Mistakes

| Mistake | Correction |
| --- | --- |
| Treating SOLID as mandatory layers | Apply SOLID to reduce coupling and clarify responsibility, not to add ceremony |
| Treating DRY as "no repeated lines" | Deduplicate stable knowledge or behavior; tolerate harmless repetition |
| Ignoring YAGNI for future flexibility | Build the extension point when the variation is real |
| Creating shared code too early | Keep code local until reuse is proven and the abstraction has a name |
| Refactoring while changing behavior | Split structural cleanup from behavior changes when practical |
| Designing from scratch in an existing codebase | Follow local architecture unless there is a concrete reason to change it |
