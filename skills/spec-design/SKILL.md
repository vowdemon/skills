---
name: spec-design
description: Use when turning a PRD, rough requirement, or product idea into a feature-level specification with concrete behavior, user flows, acceptance criteria, edge cases, and BDD-style scenarios, before writing code plans or implementation details
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# Spec Design

## Overview

Turn product intent into a precise feature-level specification. A good feature spec sits between PRD and code planning: it explains the exact behavior of one feature, the rules it must follow, the scenarios it must support, and how to verify it works.

## Position In The Stack

Use this skill when the input is too broad for implementation planning but too vague for coding. The usual flow is:

- PRD: why this feature exists and what product outcome it serves
- Spec Design: what the feature must do, in what situations, and how to verify it
- Code Plan: how to build it in this codebase

## Scope Check

Before writing the spec, decide whether the request is actually one feature or several independent features.

Split the work when:

- each part has a different user goal
- each part needs a different set of rules or states
- each part could be released or tested independently
- the document would need separate acceptance criteria for each part

If it is one feature with multiple states or branches, keep it in one spec.

## First Pass

Before drafting, answer these questions:

1. What exact behavior should this feature produce?
2. Who uses it?
3. What triggers it?
4. What does success look like?
5. What are the important states, transitions, or branches?
6. What is out of scope?
7. What still needs product or design decisions?

If any answer is unclear, ask before inventing details.

## Required Sections

A useful feature spec usually includes:

- Title
- Short summary
- Goal
- Scope
- Non-goals
- User scenarios
- Behavior rules
- State transitions or lifecycle, if relevant
- Edge cases
- Acceptance criteria
- Dependencies and assumptions
- Open questions

## BDD Style

Use BDD-style scenarios when behavior needs to be unambiguous.

Prefer one of these forms:

```text
Given [context]
When [action]
Then [expected result]
```

or

```text
Precondition: ...
Action: ...
Expected: ...
```

Use scenarios to cover:

- main success path
- error path
- empty or missing data
- boundary conditions
- repeated actions
- recovery after failure

## Writing Rules

- Write behavior, not implementation.
- Keep requirements observable and testable.
- Use one requirement per bullet when possible.
- Keep terms consistent across the document.
- Avoid vague phrases like "support", "handle", or "improve" unless the exact meaning is defined.
- Do not describe file structure, class names, or functions unless they are explicit constraints from the source material.
- Include concrete examples where they reduce ambiguity.
- Prefer states and rules over narrative prose when the feature has branches.

## Acceptance Criteria Rules

Acceptance criteria should be specific enough that someone can tell whether the feature is done.

Good acceptance criteria:

- name the visible behavior
- define the condition that triggers it
- define the expected result
- describe what should not happen when relevant

Weak acceptance criteria:

- "works as expected"
- "is user-friendly"
- "should be intuitive"
- "handles errors properly"

## Edge Case Rules

Do not hide edge cases in prose. Call them out explicitly when they matter:

- empty state
- duplicate input
- repeated trigger
- stale data
- invalid state
- partial failure
- race conditions or conflicting actions

## Review Checklist

Before finalizing the spec, check that it answers:

- What exactly is the feature?
- When does it start and end?
- What states does it have?
- What happens on success?
- What happens on failure?
- What is explicitly not included?
- What scenarios prove it works?
- What questions still block implementation?

## Red Flags

Stop and revise when you notice:

- The document still reads like a PRD instead of a feature spec.
- The document already contains implementation steps or file names.
- The feature is actually several unrelated features.
- The behavior cannot be tested or observed.
- The same rule is described in multiple incompatible ways.
- Open questions are being disguised as requirements.
- BDD scenarios are missing for the critical path.

## Common Mistakes

| Mistake | Correction |
| --- | --- |
| Staying too high-level | Add concrete behavior, states, and acceptance criteria |
| Drifting into code plan | Remove implementation details and keep the spec at the behavior layer |
| Mixing several features | Split into separate specs |
| Forgetting failure paths | Add error, empty, and recovery scenarios |
| Using vague language | Rewrite with observable outcomes |
| Copying PRD language unchanged | Translate product intent into feature behavior |
