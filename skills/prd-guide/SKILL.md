---
name: prd-guide
description: Use before and throughout any design or brainstorming work. Clarify the request, then explicitly align every proposed function and consequential rule with the user before placing them in a final design, whether or not a PRD is requested.
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# PRD Guide

Use this skill as a continuing guide to understanding requirements and judging design choices. It is not a mandatory PRD template or a phase to finish before other work begins. Before making consequential design choices, understand the user's task well enough to explain what the proposed capability must accomplish. Revisit that understanding when a later choice exposes a gap or conflict. Scale the depth to the request; a small, well-specified change may need only a brief check.

## Establish what is known

Read the request and available context. Establish what the user wants delivered and at what level of detail before choosing a document form or design method. For example, “design a page” may mean a product-level page plan, UI/UX design, visual mockup, or implementation; the phrase alone does not settle that choice. When this choice determines whether UI/UX work is in scope, clarify it before platform or layout. Distinguish requirements the user has stated, choices the user has delegated, deductions supported by context, and decisions still open. Preserve the user's chosen scope and solution where they are compatible with the stated goal. Missing product material is uncertainty, not evidence for a default; a request to create a design or document does not delegate unstated product choices.

If the brief leaves the deliverable or a product-defining choice open, make the first user-facing response one concise question about the most consequential unresolved choice. Consider the requested artifact and fidelity, intended users and their tasks, required or excluded information, available or excluded actions, target platform when it affects the result, relevant constraints or an existing design system, and the scope of creative freedom when deciding what to ask. These are areas to inspect, not a questionnaire to send all at once. Ask only about choices whose answers could materially change the result. Give concrete options and, when there is a basis for one, a conditional recommendation; do not commit to an option before the user responds.

Ask one question per message and wait for the answer. Incorporate everything the user answered, then decide whether another consequential question remains and which one now matters most. Do not send a predetermined sequence, re-ask settled choices, or treat silence as agreement. Delegation allows the agent to propose a choice, but the proposed scope still needs explicit alignment before it becomes a final design.

**Decision gate:** When an open choice would materially determine the intended users, task, information, actions, rules, scope, or requested artifact, stop before drafting or editing the design that depends on it. Continue read-only investigation if it may resolve the choice, then ask the user if it remains open. Do not complete a design around one selected option and label it an “assumption” or “open question” afterward. Existing product context and industry examples may inform a proposal, but they do not replace alignment with the user. Local, reversible details that do not change the product definition can be decided within the aligned scope.

## Understand the demand from several angles

Use the following questions to guide reasoning and later decisions, not as sections that every response must contain:

- **People and context:** Who needs this, in what situation, and what are they trying to accomplish? What prevents them from doing it now? Distinguish the primary task from adjacent use cases.
- **Information and actions:** What must people know, provide, inspect, or change to complete that task? What information or actions are required, optional, or expressly excluded?
- **Capability and behavior:** What concrete capability serves each task? Identify its entry conditions, meaningful inputs, outcomes, limits, and important failure cases. Replace feature names such as “management” or “support” with behavior that can be explained and checked.
- **Minimum useful scope:** Which capabilities together let the intended users complete the current task end to end? If a proposed part is removed, can they still do so? Include necessary recovery and constraints; exclude additions that serve only speculative use cases.
- **Boundaries and tradeoffs:** Which users, scenarios, actions, and outcomes belong in this change, and which do not? Where goals compete, identify the constraint or user consequence that decides the tradeoff. Do not enlarge the assignment merely to make the design appear complete.
- **Future change:** Which plausible later needs have a stated basis in the request or existing context? Preserve an affordable path for those changes when it affects today's decision, but do not implement future features without a current reason. Name the assumption behind any proposed extension point and its cost to the present design.

Keep the current need and future flexibility in balance. A minimum feature set is not just the shortest feature list: it must support a complete, useful task. Future readiness is not a feature wish list: favor clear boundaries and choices that can be revised when the future need becomes real.

## Align every function before the final design

Before drafting or editing the requested design artifact, present a complete, reviewable scope inventory in the conversation. List every function and user action proposed for this work, along with consequential information, rules, states, and exclusions. Make each item specific enough for the user to accept or change; distinguish the user's stated requirements from the agent's recommendations and show where external references informed a recommendation. Ask one clear question inviting confirmation or corrections to the enumerated scope. An explicit acceptance of the complete list aligns each listed item; an unlisted feature has not been approved.

After the scope is aligned, present the consequential design direction and tradeoffs at a level the user can review, and ask for alignment before drafting the affected artifact. Scale this proposal to the task; a narrow change may need only a few sentences. Approval of the feature inventory does not approve a materially different design approach.

If the user changes the list, update it and confirm the affected scope before finalizing. If a new function or consequential rule emerges during UI, technical, or interaction design, bring that item back for alignment before adding it to the artifact. Do not infer approval from silence, from a general request to design, from an industry pattern, or from the fact that a similar product has the feature.

Do not deliver a completed design containing “to be confirmed,” unresolved product questions, or conditional behavior presented as the selected solution. Keep such questions in the conversation and wait for decisions. If the user explicitly requests an exploratory comparison rather than a final design, present alternatives as alternatives and do not label one as an approved product definition.

## Make the result verifiable

Translate consequential requirements into acceptance criteria that another person can interpret and execute. For each criterion, make clear the relevant actor or situation, preconditions, action or event, and observable result. Include boundary or failure behavior where it changes whether the task succeeds. Use concrete examples when they resolve ambiguity; do not mistake an example for the full rule. Avoid criteria such as “intuitive,” “robust,” or “works well” unless their observable meaning is defined.

Trace each important capability to the aligned scope item and user task it serves, and to a way of checking its result. Resolve any missing product decision before finalizing its criterion or dependent design. Distinguish acceptance of the product behavior from an implementation or test plan; specify technical mechanisms only when they are actual constraints or necessary to explain the required outcome.

## Handle UI and UX only where relevant

Capture UI or UX requirements when the user supplies them or they materially constrain the product definition. If the brief includes an interface, an existing design system may be a relevant constraint to clarify. Otherwise, do not require visual style, layout, or interaction decisions as part of this guide. The understood users, tasks, information, actions, boundaries, and acceptance criteria should remain available to guide later UI and UX work.

## Apply the guide to the requested work

Carry the settled requirements into the design, brainstorm, review, or implementation the user actually requested. Use a PRD only when it is requested or genuinely useful as an artifact; choose a structure and level of detail proportional to that purpose. When reviewing a proposal, identify where it serves or contradicts the established task, scope, and criteria. Surface unresolved decisions at the point they affect the work, and do not claim a design satisfies an acceptance criterion that has not been checked.
