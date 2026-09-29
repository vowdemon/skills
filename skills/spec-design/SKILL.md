---
name: spec-design
description: Use when the requested work is to write, revise, or explicitly review a specification document, including an OpenSpec specification. Do not invoke merely because a product idea, feature, bug, design, or implementation task could be described in a spec.
license: MIT
metadata:
  author: vowdemon
  version: "2.0"
---

# Spec Design

## Overview

When a specification document is the requested deliverable, turn the requirements and scope aligned with the user into a stable specification that can guide multiple implementations toward the same business behavior. Describe the capability in enough detail to reproduce its intended design without binding it to current code structure or technology choices. Source material alone does not call for a spec; a page design or other requested artifact should keep its own form.

Use this skill for a complete capability specification. Keep change proposals, code plans, implementation tasks, and low-level technical designs separate.

## Specification Model

The specification sections have distinct responsibilities:

- `Purpose` explains why the capability exists, its basic usage, responsibilities, and scope.
- `Design` defines the stable, implementation-independent design of the capability.
- `Requirements` state the observable behavior that must hold.
- `Scenario` blocks verify and constrain the design through BDD examples.

Keep these sections consistent. If a design detail affects observable behavior and must remain stable, cover it with a requirement and at least one scenario. If a requirement introduces behavior absent from the design, update the design. Resolve conflicts with the user before writing the affected specification; do not silently choose an interpretation.

## Scope Check

Before drafting, decide whether the request describes one coherent capability. Split it into separate specs when parts have different goals, rules, lifecycles, or can be released and verified independently. Keep multiple states or branches in one spec when they belong to the same capability.

Before drafting, align every function and consequential rule that will appear in the specification with the user, as described in `$prd-guide`. Clarify missing information that changes the purpose, design, requirements, or scenarios. External examples and familiar patterns can inform a proposal but cannot authorize a requirement. Do not invent behavior or leave unresolved decisions in a completed specification.

## Output Contract

Use this structure:

```markdown
# <Capability> Specification

## Purpose

<Explain the purpose, basic usage, responsibilities, and scope.>

## Design

<Describe the stable, implementation-independent design in sufficient detail
for different implementations to reproduce the same business behavior.>

### <Optional design topic>

<Organize substantial design details under descriptive level-three headings.>

## <Optional supporting section>

<Add only when the spec needs an independent kind of explanation that does not
fit naturally in Purpose, Design, or Requirements.>

## Requirements

### Requirement: <Behavior name>

The system SHALL <one observable and verifiable behavior>.

#### Scenario: <Concrete case>

- **GIVEN** <initial state>
- **WHEN** <action or trigger>
- **THEN** <observable outcome>
- **AND** <additional outcome, when needed>
```

Keep `Purpose`, `Design`, and `Requirements` as level-two headings. Keep requirement headings in the exact form `### Requirement: ...` and scenario headings in the exact form `#### Scenario: ...` so OpenSpec-style tooling can recognize them.

## Purpose Rules

Write a concise introduction to the capability. Explain its intent, expected use, responsibilities, and practical scope in natural prose. Include basic usage when it helps establish the capability, such as the invocation model of a CLI.

Do not turn Purpose into a detailed design, acceptance checklist, implementation plan, or fixed list of boundary and dependency fields.

## Design Rules

Define the stable business design that should survive changes in code, framework, storage, and internal architecture. Include only topics relevant to the capability, such as:

- public interface or usage model
- core concepts and relationships
- workflows, states, and transitions
- input, output, and error semantics
- business rules and precedence
- command, option, configuration, or response behavior
- interactions with other capabilities
- compatibility or lifecycle rules

For a CLI, Design may define command grammar, command responsibilities, option semantics, configuration precedence, output conventions, exit behavior, and interaction flow. It should not prescribe parser libraries, classes, functions, source files, or internal algorithms.

Describe dependencies naturally where they affect the design. When a dependency changes observable behavior, capture that behavior in a requirement and scenario. Leave purely technical dependencies to implementation design.

## Requirement Rules

- Use one requirement for one behavior contract.
- Use `SHALL` or `MUST` for mandatory behavior.
- Use `SHOULD` only when justified exceptions are allowed, and `MAY` only for genuinely optional behavior.
- Make every requirement observable or explicitly verifiable.
- Give every requirement at least one scenario.
- Keep requirement names concise and stable.
- Put implementation mechanics in later design or planning artifacts.

Do not add a separate acceptance-criteria section. Scenarios are the executable-style acceptance criteria for their requirements.

## Scenario Rules

Every scenario must use `GIVEN`, `WHEN`, and `THEN`; use `AND` only for additional conditions or outcomes. Each scenario should describe one concrete case that exercises its parent requirement instead of paraphrasing it.

Cover the cases that materially constrain the design, including relevant success paths, invalid input, empty states, permissions, repeated actions, boundary values, dependency failures, partial failures, and recovery behavior. Do not create scenarios mechanically for cases that do not apply.

## Resolve Questions Before Delivery

Keep missing definitions and conflicting rules in the conversation while resolving them with the user. Do not add an `Open Questions`, `To Be Confirmed`, or equivalent section to a completed specification. Do not write a requirement around an unconfirmed feature or choose a default from industry practice. If a question remains unanswered, pause the affected specification work and ask; a partial working discussion is not a completed specification.

## Additional Sections

Add another level-two section only when the spec needs a distinct type of explanation that cannot fit naturally in Purpose, Design, or Requirements. Name it for its actual content and keep it at the stable specification layer. Do not add standard sections by habit or repeat information already expressed elsewhere.

Place supporting sections where they best preserve the reading flow.

## OpenSpec Compatibility

When writing or revising an OpenSpec specification, inspect its existing specs, configuration, and workflow instructions before deciding the output form. Preserve the project's distinction between current specifications and proposed changes, and keep requirement and scenario structures compatible with its tooling.

Treat Design and other optional sections as extensions that must not interfere with structured requirement blocks. Prefer project conventions over generic assumptions, and use available OpenSpec validation when appropriate.

## Bug Specifications

For a bug specification, specify the intended correct behavior rather than the implementation of the fix. Design should explain the correct execution path at the level needed to make the behavior reproducible.

Requirements and scenarios should constrain that path and cover the conditions that exposed the defect as regression behavior. Use the observed faulty behavior only as context, update existing behavior instead of duplicating it, and clarify uncertainty before finalizing.

## Review Checklist

Before finalizing, verify that:

- Purpose explains why the capability exists and what role it serves.
- Design is detailed enough to reproduce the same business behavior without relying on current code.
- Design avoids volatile implementation choices.
- Every requirement is observable, normative, and covered by a scenario.
- Every scenario uses the exact heading level and GIVEN/WHEN/THEN structure.
- Critical success, failure, and boundary behavior is covered where relevant.
- Design, requirements, and scenarios do not contradict one another.
- Every function and consequential rule has been aligned with the user, and no unresolved behavior remains in the completed document.
- The document describes one coherent capability.

## Red Flags

Stop and revise when:

- Purpose contains detailed workflows, rules, or implementation decisions that belong in Design.
- Design is too vague for another implementation to reproduce the same business behavior.
- Design prescribes current classes, functions, files, frameworks, storage choices, or internal algorithms without an explicit external constraint.
- A stable observable design rule has no corresponding requirement or scenario.
- A requirement introduces behavior that Design does not explain.
- A scenario paraphrases its requirement without a concrete initial state, trigger, and observable result.
- A scenario omits `GIVEN`, `WHEN`, or `THEN`, or uses the wrong heading level.
- Requirements and scenarios describe incompatible outcomes for the same condition.
- The document adds a separate acceptance-criteria section that duplicates scenarios.
- An unaligned function or unresolved decision appears in a completed specification.
- Optional sections repeat existing content or exist only to fill a template.
- One spec combines capabilities with independent goals, rules, or lifecycles.

## Common Mistakes

| Mistake | Correction |
| --- | --- |
| Treating Purpose as a complete feature description | Keep Purpose concise; move stable behavioral detail into Design |
| Writing Design as a code plan | Describe durable interfaces, concepts, workflows, rules, and semantics |
| Keeping Design at summary level | Add enough detail for independent implementations to reproduce the same business behavior |
| Copying Design paragraphs into Requirements | Extract the specific observable obligations that must remain true |
| Using scenarios to introduce the design | Define the behavior in Design first, then verify it with requirements and scenarios |
| Restating a requirement as its scenario | Give the scenario a concrete context, trigger, and outcome |
| Creating a separate acceptance-criteria list | Use scenarios as the acceptance criteria for each requirement |
| Adding fixed boundary or dependency sections by habit | Explain relevant context naturally and add a section only when the content needs one |
| Describing a dependency only by name | Define its observable effect in Design and cover failure behavior when it matters |
| Guessing through ambiguity | Ask the user to resolve the exact missing or insufficient definition before drafting the affected behavior |
| Adding unresolved questions to the final document | Keep clarification in the conversation; deliver the specification only after its behavior is settled |
| Adding every possible edge case | Cover cases that materially constrain the design and omit irrelevant ceremony |
