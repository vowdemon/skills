---
name: code-review
description: You MUST use this after any code design or implementation. Review behavior and architecture in code changes, modules, and design proposals, including code reviews, refactoring assessments, and design evaluations at any stage. Examine contracts, naming, responsibilities, state, lifecycles, and module boundaries.
license: MIT
metadata:
  author: vowdemon
  version: "1.0"
---

# Code Review

Review whether behavior satisfies its contracts and whether the structure assigns responsibility, maintains invariants, and contains change. Express intent accurately, reduce unnecessary cognitive load, and preserve the complexity needed for correctness and reliability.

Behavior and architecture both carry review obligations. Development stage changes emphasis, not whether either dimension matters. Architecture has independent preventive value: a demonstrated false concept, responsibility leak, or missing constraint does not need a runtime incident to become actionable.

Before delivery, review the resulting proposal or change at a depth proportional to the work. Resolve substantiated issues within the authorized task before reporting completion.

## Core Judgment

- **Understand semantics before judging shape.** Read relevant calls, data flow, lifecycles, and contracts. Names, line counts, repeated syntax, layers, and branch counts alone do not prove a defect.
- **Distinguish problems from preferences.** Report concrete effects on correctness, reliability, comprehensibility, encapsulation, change cost, or established goals. Do not require every design issue to be disguised as a bug.
- **Trace causes without assuming a larger defect.** A misleading name may need only renaming; repeated local failures may reveal a shared owner or boundary problem. Find the responsibility that explains and can correct the established issue.
- **Judge total responsibility, not local polish.** Splitting or inlining must not merely distribute knowledge, hidden protocols, or synchronization elsewhere.
- **Make complexity earn its cost.** Constraints, isolation, compatibility, performance, and lifecycle guarantees can justify structure. Necessary repairs can increase code or local complexity; reject unnecessary complexity, not necessary guarantees.
- **Respect effective project decisions.** Account for language, framework, scale, and architectural intent. Existing patterns are evidence, not automatic authority; project conventions do not erase demonstrated defects.
- **Prevent supported problems early.** Correct current design mismatches and constraint gaps, including their use as templates for further implementation. Do not invent future requirements or claim that possible downstream damage has already occurred.
- **Recommend the smallest complete repair.** Restore the violated contract across established same-cause paths, then exclude unrelated change. Minimize unnecessary change, not changed lines.

## Scope, Stage, and Contracts

Identify the scope and stage from the request and relevant materials. State consequential assumptions; ask only when an unresolved choice materially changes the review. A mature repository can contain a newly designed subsystem.

| Scope | Review boundary |
|---|---|
| Design or interface proposal | Intended responsibilities, constraints, dependencies, and key success/failure scenarios |
| Change or PR | Changed behavior and structure plus necessary callers, callees, persisted formats, and affected contracts |
| Module or subsystem | Internal invariants and actual cross-boundary use |
| Whole project | Risk distribution, important end-to-end paths, and explicit coverage depth |

For changes, distinguish introduced, worsened, and pre-existing issues. Follow the requested treatment of existing defects; do not attribute all discovered debt to the change.

| Stage | Primary emphasis | Concurrent obligation |
|---|---|---|
| Design | Requirements, vocabulary, ownership, constraints, and intended boundaries | Validate key behavior, failure, concurrency, and recovery scenarios against the proposal |
| Early implementation | Faithful realization of intent; prevent unsupported concepts and wrong interfaces from becoming foundations | Check implemented paths, state transitions, and irreversible effects |
| Completed implementation | Actual correctness, failure recovery, compatibility, and regression | Check structural causes, constraint gaps, and change propagation |
| Incident response | Contain impact and restore service | Distinguish mitigation from the work needed to close the root cause |

Do not assign mechanical time percentages or infer priority from stage alone. Follow serious evidence wherever it leads.

Read relevant requirements, acceptance criteria, project instructions, architecture decisions, interface contracts, implementation, and tests. Distinguish explicit requirements, contracts inferred from legitimate use, and review assumptions. Check applicability and accepted exceptions. Conflicting or stale documents and tests require reconciliation; do not silently select whichever supports a finding.

Build only the system model needed for this scope: inputs and outputs; important invariants and when they must hold; state and resource owners; mutation and commit points; irreversible effects; and changes the boundaries are intended to absorb.

## Design Checks and Reference Routing

Keep the following checks active throughout review. Read the linked sections when assessing the corresponding area; do not wait for a behavior bug to trigger design reading. A small scope may need only a few sections. A broad review should examine the applicable areas and record any important omissions.

| Area | Minimum check and detailed guidance |
|---|---|
| Naming | Truthful concepts and verbs; meaningful distinctions; no unsupported nouns or symmetry. Trace foundational vocabulary for [naming contagion](references/design-principles.md#naming). |
| Responsibilities | Preserve complete concepts; distinguish orchestration from rule ownership; share knowledge rather than similar syntax. Check [cohesion, ownership, protocols, and change locality](references/design-principles.md#responsibilities-and-cohesion). |
| Abstractions | Each layer, generic, and option must carry semantics or a guarantee. Keep deletion as an option without erasing useful isolation. Check [configuration, symmetry, and sufficient design](references/design-principles.md#abstractions-and-complexity). |
| Control flow | Make decisions, transitions, ordering, and loop progress understandable; do not mechanically flatten or extract. Check [paths and failure boundaries](references/design-principles.md#control-flow). |
| State | Classify authority, derivation, caches, and drafts before judging copies; maintain invariants at write boundaries rather than through scattered synchronization. Check [data flow](references/design-principles.md#state-and-data-flow). |
| Effects and lifecycles | Identify owners, validity, cleanup, and repetition policy; cancellation is not proof that stale results cannot commit. Check [lifecycle guarantees](references/design-principles.md#side-effects-and-lifecycles). |
| APIs and modules | Expose minimal complete capabilities, constrain invalid use, distinguish semantic from signature compatibility, and keep shared concepts owned. Check [contracts and dependency direction](references/design-principles.md#api-and-module-boundaries). |
| Comments | Preserve necessary intent and contracts; correct misinformation and repetition without deleting rationale. Check [comment quality](references/design-principles.md#comments). |

## Execution

### Select risk and trace both dimensions

Prioritize relevant risks: identity/data isolation, irreversible effects, mutable authority, asynchronous commits, partial failure, public contracts, duplicated rules, and foundational interfaces that are spreading. Adapt to the actual system instead of applying an exhaustive generic checklist.

Trace behavior as **entry → preconditions → decision/state transition → effects → result/commit → failure and recovery**. For stateful, asynchronous, externally interacting, or compatibility-sensitive paths, read the relevant sections of [Behavioral Verification](references/behavioral-verification.md). That reference also supplies scenario-based checks for designs with no executable implementation.

Trace structure as **intended constraint → knowledge and responsibility owner → public capability → actual use/dependency → guarantee and change propagation**.

Use behavior to locate structural causes, and structure to find unexamined behavior paths. Do not postpone all architecture work until behavior review is finished, or let extensive design analysis substitute for tracing actual execution.

### Verify candidates and counterevidence

For each candidate, establish its basis, causal mechanism, applicable conditions, consequence, relevant protection, and corrective responsibility. Check callers and callees, guards, framework guarantees, cleanup, recovery, and accepted tradeoffs that could refute the candidate. Verify uncertain external API or version behavior from applicable implementation or documentation.

Use tests or experiments when they resolve a material uncertainty. A complete static argument can establish a finding; merely running tests does not establish its causal claim. Record what a check actually demonstrates.

### Apply the evidence gates

**Behavior finding:** identify the expected contract, a legitimate entry and trigger, the incorrect path or transition, observable consequence, and why relevant protection does not prevent it. Legitimate use includes libraries, build systems, and developer tools, not only production UI. Do not extrapolate a test-only failure to production without evidence.

**Architecture finding:** identify an effective constraint, present responsibility, or specific quality goal; show the structure and its mechanism of false classification, responsibility leakage, missing enforcement, or change amplification; identify affected current use or established requirements and the corrective boundary. Neither written architecture rules nor an existing runtime failure are mandatory.

A surface feature is a lead. A demonstrated design mismatch is evidence. For example, an early name used to justify a split that separates one invariant is reviewable now; a suffix alone does not prove that the system will accumulate debt.

If an unknown fact determines whether the defect exists, keep it as an unresolved lead. If the defect is established but its reach or frequency is uncertain, report the established scope and qualify the rest. Do not launder speculation into a confirmed finding with a low-confidence label.

### Resolve findings and repair scope

Retain every independent, substantiated, actionable issue; do not aim at a finding count or a behavior/architecture quota. Group by a specific shared cause and cohesive correction, not by module, principle, or the possibility of one large rewrite. Do not count one cause twice as separate behavior and architecture issues.

Read [Findings and Remediation](references/findings-and-remediation.md) when deciding disputed grouping, priority, uncertainty, or repair sufficiency. Its priority definitions apply unless the user or project supplies another scheme.

Use P0–P3 for action priority: immediate catastrophic issues; important issues before the relevant delivery or design commitment; substantive issues to schedule; and limited local issues. Assess impact, reach, likelihood, recoverability, established goals, and specific repair windows separately from evidence strength. Architecture is not automatically P3, and cheap repair alone does not justify escalation.

A complete repair restores the contract at the responsible boundary across established same-cause paths. Check whether callers can still bypass it, whether legitimate use remains possible, and whether the recommendation introduces unrelated changes. State the required invariant and acceptance criteria when evidence does not support choosing an implementation. Necessary structural correction is part of repair. Mitigation must identify the remaining cause and conditions for closure.

For a design or architecture finding, use `$code-design` to shape the repair recommendation around the demonstrated cause and established constraints.

## Coverage and Completion

For a small change, keep a brief account of key calls, boundaries, checks, and limits. For a module or project, maintain a lightweight record of included/excluded areas, relevant entry points, depth of examination, behavior/design coverage, candidate dispositions, verification, and unresolved high-risk areas. Keep it proportional; it need not be a separate deliverable.

Distinguish deep tracing, local reading, and search-only coverage. Review is complete for a stated scope only after the planned important paths and boundaries are examined and high-risk candidates are resolved or explicitly left unverified. Reading every file is not proof of semantic coverage. Finding count is never a stop condition.

When time, tools, or environment prevent completion, narrow the completion claim and identify consequential gaps. Do not claim to exhaust all possible defects.

## Output

Adapt to the user's or project's format. By default provide:

1. Scope, stage, and consequential assumptions, briefly.
2. Findings ordered by action priority, with location, causal evidence, impact, and complete repair direction. Include relevant uncertainty and verification without forcing a long field template.
3. Concrete unresolved leads worth further investigation, when any.
4. Coverage, validation results, and material limits.

A finding should be understandable without the review conversation. Cite the actual defect or contract location and supporting paths where necessary. Preserve all independent actionable findings while removing repetition and unsupported preferences. If none are established, say so within the reviewed scope.

Do not claim that a recommendation was implemented or validated unless it was. Review itself does not authorize code changes; honor any repair authorization already present in the task.
