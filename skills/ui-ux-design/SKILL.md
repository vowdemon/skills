---
name: ui-ux-design
description: Use when the requested work explicitly involves interface flows, layout, visual design, UI review, or implementation of a specified interface. Design for the established tasks, scope, and constraints.
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# UI/UX Design

Design an interface for the task people need to complete. Apply only the parts of this guide relevant to the requested deliverable; a flow, layout, visual design, review, and implementation need different evidence and detail. Do not turn this skill into a fixed document template.

## Establish the work and its inputs

Identify whether the user wants a new UI/UX design, a review, or implementation, and the expected fidelity of the result. Use `$prd-guide` to clarify an ambiguous deliverable before producing interface detail. If the request is for product-level page planning rather than UI/UX work, keep the output at that level.

Use the established product scope for intended users and tasks, required or excluded information and actions, capability boundaries, and constraints. Inspect the existing interface or design system when relevant. Use the decision criteria in `$prd-guide` if an interface choice exposes a consequential gap in product scope.

Establish the relevant platforms, input methods, and viewport ranges from the brief or product context. Clarify an unresolved platform choice when it would materially change the design and the available context gives no sound basis for choosing.

Distinguish user requirements, observed behavior, applicable platform conventions, supported deductions, assumptions, and design recommendations. Use conventions and examples as evidence for recommendations within scope. Report user research, accessibility results, and platform behavior at the level the available evidence supports.

## Resolve interface choices

Identify choices that materially change what people see, understand, do, or recover from. Consider information hierarchy, navigation, interaction patterns, important states, and visual direction when relevant. Use established decisions and choose delegated details within scope.

For a new or materially changed interface, show the proposed information hierarchy, main interaction, and important states and consequences at the fidelity the user requested. Explain consequential choices and use the existing brief as the basis for reviews and narrowly specified changes.

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

For a design deliverable, present the functions and decisions in scope with the reasoning needed to understand them. Label recommendations and any open dependency that limits the result. For a review, explain each finding through the user consequence and the observed or projected mechanism. Prioritize blocked tasks, harmful actions, exclusion, and hard recovery. Preserve unaffected behavior unless a concrete benefit justifies changing it.

For implementation, carry settled meanings, states, transitions, feedback, semantics, and visual behavior into the interface. Inspect the rendered and interactive result on the relevant platform and viewport. Source code or passing structural tests alone do not establish visual hierarchy, focus behavior, or interaction fidelity. Report what was actually verified and keep unsupported claims conditional.
