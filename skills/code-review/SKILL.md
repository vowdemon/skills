---
name: code-review
description: You MUST use this once before delivery after completing a code design or implementation batch, and when asked to review code, a change, module, or design. Review correctness and whether code earns its complexity, including unnecessary defenses, obsolete paths, indirect implementations, duplicated responsibility, and speculative abstractions.
license: MIT
metadata:
  author: vowdemon
  version: "1.2"
---

# Code Review

Review both **whether the code works** and **why this implementation needs to exist**. A useful review catches broken behavior and removes substantiated burdens in code that already works. Preserve guarantees supplied by validation, isolation, compatibility, performance, and lifecycle management.

Use naturalness as a discovery signal. Under the current requirements and constraints, does the implementation express the operation directly? Every extra state, branch, conversion, protocol, and abstraction should have a concrete job. An awkward path invites investigation. Evidence determines whether it needs correction.

## Set the scope

Read the request, project instructions, relevant contracts, and change. Resolve contradictions with actual supported use. Distinguish requirements, inferred contracts, and assumptions. Ask only when a material choice cannot be resolved from context.

- For a small or medium PR, walk the changed first-party files, reading complete affected functions and their surrounding responsibilities. Include obsolete code left behind by the change. Follow callers, callees, and persisted formats as needed.
- For a large change or whole project, map the subsystems and choose bounded areas for detailed reading, including important behavior paths and areas of accumulated complexity. State sampling and coverage limits. Generated or vendored files usually need contract checks at their boundaries.
- For a design, apply the same questions to the proposed operations, concepts, and constraints. Early work needs scrutiny of foundations. Completed work also needs failure, recovery, and compatibility tracing. During an incident, prioritize containment while identifying the remaining cause.

Keep this boundary stable for one discovery pass. Trace extensions needed to establish or refute a candidate. Distinguish introduced, worsened, and pre-existing problems according to the requested scope.

## Read for intent and necessity

For each selected file or coherent unit, establish its required result and constraints from legitimate callers and requirements. Outline the direct operation that would satisfy them, then compare its necessary decisions, state, and effects with the implementation's concepts, representations, branches, and handoffs. For each difference, identify the concrete guarantee or supported use it serves. A necessary responsibility does not justify every mechanism used to implement it; an unexplained difference is a candidate to investigate.

Use this comparison within the selected reading. A short expression or flow is enough when an alternative needs to be made explicit; do not rewrite every function or add a separate full scan. Fewer lines or a familiar pattern alone establishes neither simplicity nor necessity.

Use the six lenses below to notice candidates during this reading. A file can have several independent issues or none. Do not wait for a behavior failure before examining the code's necessity.

| Lens | Questions that generate candidates |
| --- | --- |
| Excessive defense | Is this condition already guaranteed on every supported entry? Does the guarantee survive mutation or an asynchronous gap? What does this fallback or catch accomplish? Look for repeated validation, impossible guards, error masking, and compatibility branches without a supported input. |
| Obsolete or unreachable code | What still calls this implementation? Can this branch be reached after earlier validation or returns? Has the new path replaced its role? Check entry points, dynamic discovery, supported old data and versions before judging absence of use. |
| Indirect implementation | Follow one input to its result. Which wrappers, forwarding steps, intermediate representations, or conversions add no semantics? Can the operation be expressed directly while preserving order, errors, effects, and boundaries? |
| Logic and correctness | Does each decision or transition satisfy its contract? Trace boundaries, rejected operations, loop progress, state updates, and effect ordering. Follow the important failure and recovery paths as well as success. |
| Duplicate state or responsibility | Where is each fact authoritative, and who owns each rule? Are multiple writers synchronizing a derivable value or independently deciding the same policy? Distinguish necessary snapshots, caches, replicas, and drafts by their validity and ownership. |
| Speculative complexity | What current or concretely approved variation needs this interface, registry, factory, mode, or configuration? What guarantee does the boundary provide? If a fixed operation is represented as a configurable framework, compare the caller obligations and concepts with a direct implementation. |

Also examine truthful naming, cohesion, module/API boundaries, comments, and change locality. A misleading foundational concept, hidden caller protocol, or misplaced rule can be a finding in its own right. Similar syntax alone does not establish shared responsibility.

Keep brief candidate notes as you read so structural concerns survive the investigation of urgent bugs. Suspicion is enough to record a candidate. It is not enough to report one. Complete the selected file/unit reading even when the first few behavior bugs are already clear.

## Verify candidates

For each candidate, check the explanation that could justify the existing code. Trace the responsible boundary and actual use. Classify it as established, refuted, or unresolved with a specific missing fact. Stop investigating when that disposition is supported.

**For a behavior finding**, establish the expected contract, legitimate trigger, incorrect transition or effect, observable consequence, and why existing protection fails. For stateful or external operations, trace entry → decision → mutation/effect → commit → failure/recovery. Check relevant identity isolation, repeat invocation, partial completion, and persisted state. For concurrency, construct an interleaving and inspect who can still commit after cancellation, replacement, or teardown. Check resource ownership and release on relevant exits.

**For a complexity or architecture finding**, establish all three of the following.

1. The responsibility, guarantee, or supported use that applies here.
2. The concrete burden in the present implementation, such as duplicate rule ownership, a false concept, an unnecessary representation or coordination step, or a path that cannot execute.
3. A complete correction at the responsible boundary. For a simplification, show a more direct shape and why it preserves required behavior and useful boundaries. Show a small replacement expression or flow when that makes equivalence clear. A missing guarantee may instead require additional structure.

When a simplification changes behavior, establish whether the difference serves a supported caller, persisted format, compatibility obligation, or intended new requirement. Compare with the baseline where available; current behavior and historical behavior are evidence, not automatically requirements. Preserve demonstrated obligations, including undocumented reliance. Neither the mere existence of a difference nor the absence of documentation settles its status; identify any material missing evidence.

Ask what would actually be lost by deleting the guard, deriving the value, collapsing the conversion, or removing the abstraction. One implementation can still justify an interface through isolation or lifecycle ownership. Validation at an input boundary and a database constraint can protect different entry paths. A cache can earn its storage through measured cost and a sound validity policy.

Confirmed unnecessary code has a present maintenance cost even when output is correct. Explain the redundant decision, representation, or coordination the maintainer must understand and keep consistent. Avoid invented incidents or unsupported claims about future growth.

Use a targeted test or experiment when it resolves uncertainty. A static proof is sufficient for a validated impossible branch or equivalent simplification. Verify uncertain framework or external API semantics against the applicable implementation or documentation. A missing dependency is a verification limit. Passing existing tests supports only the scenarios they exercise.

## References when needed

The checks above are sufficient for ordinary local findings. Read only a relevant section of a reference when a candidate or consequential coverage question needs deeper reasoning. Use its contents and headings to select that section.

- [Design principles](references/design-principles.md) supplies detailed naming, cohesion, abstraction, control-flow, state, lifecycle, API, and comment checks.
- [Behavioral verification](references/behavioral-verification.md) supplies failure, concurrency, resource, and compatibility scenarios when those guarantees need deeper tracing.
- [Findings and remediation](references/findings-and-remediation.md) resolves disputed grouping, uncertainty, priority, or repair completeness.

Use `$code-design` when a repair requires a substantive ownership or interface decision. A proven local deletion or direct expression can be explained in the finding itself.

## Report and close the review

**Report every independent, established issue within the reviewed boundary.** Behavior and maintenance issues have equal standing for inclusion. Once an issue meets its evidence gate, do not impose another “worth reporting” threshold based on its category, code size, rarity, severity, or the presence of more serious findings. Priority determines handling order, never whether an established issue is reported.

Group manifestations only when they share a specific cause and cohesive repair. After fixing one issue, could the other still remain? If so, preserve the distinction. Do not merge independent simplifications into a vague “clean up this module” comment. Finding counts and category quotas are not completion criteria.

Unless the project supplies a scheme, use P0 for immediate catastrophic issues, P1 for important corrections before the relevant delivery or design commitment, P2 for substantive issues to schedule, and P3 for local corrections. Priority follows impact, reach, likelihood, recoverability, and the correction window. Architecture can be urgent. Limited dead code can merit P3. Uncertain existence stays an unresolved lead.

Adapt to the requested format. Give the scope and consequential assumptions briefly, then findings ordered by priority with file/line, causal evidence, concrete impact or maintenance burden, and the smallest complete repair direction. Include significant unresolved leads with the missing evidence and a concise account of coverage and verification. Distinguish deep tracing from local reading and search-only coverage. Name material omissions.

Before finishing, reconcile the candidate notes with the output: every established issue has a finding or an explicit same-cause grouping; excluded candidates have a scope or counterevidence reason; unresolved candidates name the missing fact. Shorten explanations and remove repetition to meet length limits; do not silently drop independent findings. Finish when the selected files, important paths and boundaries, and their candidates have been covered. Do not restart general discovery after verification, or claim exhaustive coverage from file counts.

Review alone authorizes recommendations. When repair is already authorized, make the smallest complete correction at the responsible boundary, including established same-cause paths and legitimate old callers/data. For state, concurrency, or lifecycle repairs, define the invariant, owner, participating actors, ordering, and commit point before editing. Prefer eliminating invalid states or duplicated coordination. Each added flag, lock, token, or queue must enforce a distinct guarantee.

Verify repaired causes and affected legitimate behavior. If a repair introduces another failure of the same class, replace or redesign the mechanism instead of layering compensating patches. Keep verification focused on the repair and its direct effects. Start another full pass only when requested or when the repair materially changes the contract or scope. State remaining constraints if a stable repair cannot be established, and distinguish proposed, implemented, and verified work.
