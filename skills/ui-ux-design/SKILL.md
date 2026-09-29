---
name: ui-ux-design
description: Use when the requested work explicitly involves interface flows, layout, visual design, UI review, or implementation of a specified interface. Align every function and consequential interaction with the user before including it in a final UI/UX design.
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# UI/UX Design

Design an interface for the task people need to complete. Apply only the parts of this guide relevant to the requested deliverable; a flow, layout, visual design, review, and implementation need different evidence and detail. Do not turn this skill into a fixed document template.

## Establish the work and its inputs

Identify whether the user wants a new UI/UX design, a review, or implementation, and the expected fidelity of the result. Use `$prd-guide` to clarify an ambiguous deliverable before producing interface detail. If the request is for product-level page planning rather than UI/UX work, keep the output at that level.

Take the intended users and tasks, required or excluded information, available or excluded actions, capability boundaries, and constraints from the product scope aligned under `$prd-guide`. Inspect an existing interface or design system when it affects the request. If a product-defining choice is missing, stop before drafting or editing the interface design that depends on it and resolve it with the user. An assumption label in the finished artifact does not substitute for that decision. Do not use a layout or interaction pattern to silently define product scope.

Establish the platforms, input methods, and viewport ranges relevant to the requested result from the brief or product context. If an unresolved platform choice would materially change the design, ask before drafting the affected layout. Work within the established scope rather than adding variants to make the artifact look complete.

Distinguish user requirements, observed behavior, applicable platform conventions, supported deductions, assumptions, and design recommendations. Conventions and external examples can support a recommendation, but every function or consequential interaction they suggest still needs user alignment. Do not present a recommendation as a requirement or claim user research, accessibility results, or platform behavior that has not been established.

## Resolve interface choices interactively

Identify the UI and UX choices that materially change what people see, understand, do, or recover from. Examples include information hierarchy, navigation, interaction pattern, important states, and visual direction when visual design is in scope. Ask about the most consequential unresolved choice first, with concrete options and a recommendation when the brief supports one. Keep the recommendation conditional until the user responds.

Ask one question per message and wait for the answer before drafting or editing work that depends on that choice. Use the answer to decide whether another question remains; do not send a fixed questionnaire. A request to produce a design or a delegation to propose an approach does not approve its functions. Decide reversible local details within the aligned direction without seeking a separate approval for each one.

For every new or materially changed interface design, present a reviewable direction before the final artifact. Show how the aligned information and actions are presented, the main interaction, and important states and consequences. Check every function and consequential interaction against the scope inventory; propose and align any new item before adding it. Ask the user to confirm or correct the complete direction, then incorporate the response. For a review, report findings against the existing brief; for a narrowly specified change, stay within the aligned scope and direction and return to the user if either expands.

## Design the experience

Follow the user's task through its meaningful states: entry, decision, action, feedback, failure, recovery, and completion where each applies. Show state when people need it to choose or act. Name actions by their results and explain constraints or consequences where they matter. Keep terms and representations consistent; when wording cannot prevent a harmful misunderstanding, change the interaction or add an appropriate preview, confirmation, or recovery path.

Prevent mistakes when prevention costs less than the mistake and recovery. Preserve entered work and context after errors. Account for interruptions and blocked legitimate actions. Reserve disruptive confirmation for consequences that justify it; avoid redundant status noise.

Define semantic and focus order with the flow. Provide equivalent understanding and control across the input and assistive methods in scope. Verify accessibility properties at the fidelity where they become testable rather than claiming them from an outline or mockup.

## Design the presentation

Organize the required information and actions around their importance to the task. Use hierarchy, grouping, typography, spacing, color, and density to clarify priority and relationships. Keep representative content when scanning, truncation, localization, or comparison affects the design. Do not add information or actions merely to fill a layout.

Use an existing product or platform pattern when its meaning fits. Depart from it when a concrete task benefit justifies the learning and consistency cost. Reuse components while their established meaning remains intact. Across the platforms and viewport ranges in scope, preserve information priority and action consequences as the layout adapts; do not silently remove an action at a transition.

Use visual treatment and motion to clarify meaning, causality, or state change. Keep motion proportional and interruptible, and preserve meaning when motion is reduced or unavailable. Apply visual detail only to the degree needed for the requested result or the design decision being assessed.

## Choose evidence and check the result

Observe an existing product or analogous platform behavior when that can settle a disputed mechanism. Prototype an interaction when text or static representations cannot reveal whether it works. Compare alternatives with the task, content, environment, and unrelated choices held stable enough to isolate the decision. A prototype supports only the behavior it actually preserves; visual polish does not establish comprehension or integration.

For a design deliverable, present only the aligned functions and decisions with the reasoning needed to understand them. Do not include a “to be confirmed” section or unresolved product choices in a completed design. Keep unresolved questions in the conversation and wait for answers before finalizing. For a review, explain each finding through the user consequence and the observed or projected mechanism; prioritize blocked tasks, harmful actions, exclusion, and hard recovery. Preserve unaffected behavior unless a concrete benefit justifies changing it.

For implementation, carry settled meanings, states, transitions, feedback, semantics, and visual behavior into the interface. Inspect the rendered and interactive result on the relevant platform and viewport. Source code or passing structural tests alone do not establish visual hierarchy, focus behavior, or interaction fidelity. Report what was actually verified and keep unsupported claims conditional.
