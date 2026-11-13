---
name: prd-guide
description: Clarify product intent and consequential requirements before and during design or brainstorming. Use existing decisions and ask when an unresolved choice materially affects the requested result.
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# PRD Guide

Use this guide throughout the requested work. Start with the outcome the user needs and the level of detail they asked for. Revisit requirements when a design choice exposes a gap or conflict. A small, well-specified change may need only a brief check.

## Establish the scope

Read the request and available context. For example, “design a page” may mean a product-level page plan, an interaction design, a visual mockup, or implementation. Resolve the intended deliverable before making choices that depend on it. Distinguish stated requirements, established product behavior, delegated decisions, supported deductions, and open choices.

Use these questions as needed:

- Who needs the capability, in what situation, and what task must it let them complete?
- What must they know, provide, inspect, or change? Which information and actions are required, optional, or excluded?
- What are the entry conditions, meaningful inputs, observable results, limits, and failure or recovery paths?
- Which capabilities are necessary to complete the task? Which tradeoffs follow from the user's constraints?
- Which future needs have a concrete basis, and what does preparing for them cost today?

## Resolve consequential uncertainty

Carry forward decisions established by the request, prior discussion, and reliable project context. Make delegated and reversible choices within scope. State assumptions when they materially affect the result.

Ask a focused question when an unresolved choice materially changes the intended users, task, actions, rules, scope, or deliverable and the context and delegation give no sound basis for choosing. Explain what depends on the answer and give concrete options when useful. Continue independent work while the question is open. Use the same requirements and decisions across product, interface, and technical work. Revisit only decisions affected by new information or a change in scope.

For exploratory work, compare alternatives and identify the decision each would require. For a final design, distinguish settled behavior from recommendations and dependencies that still need resolution.

## Make the result verifiable

For each consequential requirement, identify the actor or situation, preconditions, action or event, and observable result. Include boundary or failure behavior where it changes whether the task succeeds. Connect each criterion to the capability and user task it serves. Specify a technical mechanism when it is a genuine constraint or necessary to explain the outcome.

Carry the requirements into the design, review, or implementation the user requested. Use a PRD when requested or when it helps communicate the decisions. In a review, explain where the proposal serves or contradicts the established task and criteria, and report which criteria were checked.
