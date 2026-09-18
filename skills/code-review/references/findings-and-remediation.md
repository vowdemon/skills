# Findings and Remediation

Use this reference to resolve finding boundaries, evidence strength, priority, repair sufficiency, and reporting. It does not require a fixed field template for every issue.

## Contents

- [Establish an actionable finding](#establish-an-actionable-finding)
- [Separate existence, impact, and uncertainty](#separate-existence-impact-and-uncertainty)
- [Group by cause and cohesive repair](#group-by-cause-and-cohesive-repair)
- [Calibrate action priority](#calibrate-action-priority)
- [Complete root-cause repair](#complete-root-cause-repair)
- [Mitigation, migration, and optional improvement](#mitigation-migration-and-optional-improvement)
- [Write a reviewable finding](#write-a-reviewable-finding)

## Establish an Actionable Finding

A finding establishes that, under identified conditions, a specific mechanism violates an effective contract or creates a concrete design deficiency, with an impact worth correcting. Locate the defect or erroneous contract and use callers or related paths as supporting evidence.

For behavior, connect expectation, legitimate trigger, execution/state transition, consequence, and insufficient protection. For architecture, connect a real responsibility or quality goal, structural evidence, the missing guarantee or knowledge/change burden, and the responsible correction boundary.

Present design errors can be actionable before they cause runtime faults. For example, a proposed configuration loader that owns editing state and motivates a separate saver can already misclassify one cohesive responsibility. Report the established classification and split, not an invented history of resulting production incidents.

Conversely, a suffix, duplicate line, or interface with one implementation is only a lead. An undocumented hypothetical future consumer is not enough to demand an extension point. An alternative design being possible does not make the existing design wrong.

Check the counterargument that could invalidate the finding: a guard, effective scope, framework guarantee, accepted contract, generated source, or different ownership policy. State a material unresolved assumption rather than silently relying on it.

## Separate Existence, Impact, and Uncertainty

Distinguish whether the defect exists, how far its consequences reach, and whether a particular repair is known to be sufficient.

- If a missing fact determines existence, keep a concrete unresolved lead and name the evidence needed to confirm or reject it.
- If existence is established but reach, frequency, or severity is uncertain, report the proven scope and qualify the rest.
- If the defect is established but the implementation choice is open, specify the invariant and acceptance criteria without inventing a fully worked solution.

An unresolved lead is terminal for the current review pass. It does not become a finding through repeated investigation without new evidence or an explicit request to continue.

For example, a stale request may demonstrably overwrite newer state while cross-account leakage remains unverified. Report the overwrite and do not upgrade the unverified consequence into fact.

If the requested format uses confidence, high confidence means the critical causal facts have been checked; a static proof can qualify. State the actual residual condition for lower confidence. Do not use a low-confidence label to admit unsubstantiated findings. Confidence does not replace impact or action priority.

## Group by Cause and Cohesive Repair

Merge manifestations when they share one identifiable causal defect and a cohesive correction addresses them together. Preserve distinct triggers and consequences as evidence within that finding.

Do not merge merely because issues share a module, owner, symptom, principle, or architectural theme. One large rewrite can remove unrelated problems; that does not make them one cause.

Use a counterfactual: after fully repairing one claimed defect, could the other independently remain? If so, separate them unless the apparent repairs were only partial manifestations of the same underlying defect. Different locations or triggers are clues, not automatic split rules.

Examples:

- Several entry points bypass the same owned normalization rule: one boundary defect may cover the related violations.
- One component leaks a timer and independently computes the wrong total: shared location does not make one finding.
- One session-ownership defect causes both stale publication and data lifetime mismatch: describe both without counting separate architecture and behavior findings.
- Correcting ownership stops stale session writes, but an unrelated request-ordering error remains within a session: separate causes may require separate findings even if both touch the store.

Do not hide independent issues under vague labels such as “refactor lifecycle management.” A grouped finding must still have an understandable mechanism, repair boundary, and acceptance condition.

## Calibrate Action Priority

Use the user's or project's priority scheme when supplied. Otherwise P0–P3 represents when an issue should be addressed, considering consequence, scope, likelihood, recoverability, established delivery goals, and specific expansion or migration costs.

Assign action priority only after the defect or design deficiency has passed its evidence gate. A candidate's possible severity must not be used to prove its existence.

| Priority | Meaning |
|---|---|
| P0 | Immediate action for a catastrophic issue, such as broad irreversible corruption, a severe security failure, or complete loss of a critical service |
| P1 | Important correction before the relevant delivery, merge, or design commitment; severe behavior failures or demonstrated architecture defects that block a key goal or imminently lock in a costly wrong contract |
| P2 | A definite issue to schedule, with substantive correctness, reliability, comprehension, or maintenance impact that does not require the above urgency |
| P3 | A limited local issue that remains worth correcting |

Explain the decisive factors rather than mechanically scoring every dimension. Do not confuse insufficient evidence with low severity; an unverified catastrophic hypothesis is an unresolved lead, not automatically P0 or P3.

Architecture is not capped at P3. For a high-priority preventive finding, identify the effective constraint, affected established goal, concrete propagation path, and why the current decision window matters. “It may become messy” and “it is cheap now” are insufficient alone.

For example, a public API about to be adopted by supported clients exposes a backend-specific type that prevents an already approved capability. Its correction window can matter even before a runtime failure. A local misleading private name with limited reach normally has different urgency; a name that misstates a critical operation's semantics may have more serious consequences.

Recovery by reopening a screen does not impose a fixed ceiling: consider recurrence, scope, blocked workflows, and irreversible effects before recovery. Likewise, debug-only status, resource leaks, file size, or naming category creates no universal floor or ceiling. Priority follows the actual supported use and impact.

## Complete Root-Cause Repair

Recommend the smallest cohesive change set that fully corrects the established root cause across affected paths. Minimize unnecessary change, not changed lines.

### Locate the responsible cause

Start at the concrete failure or design deficiency:

1. What condition directly creates the incorrect behavior or structural burden?
2. Why can that condition be created, observed, or committed?
3. Which related entries share the mechanism?
4. Which owner has both responsibility and enough control to maintain the constraint?
5. What correction there would eliminate the established manifestations?

Stop at a specific responsibility that explains the problem and admits a verifiable repair. “Bad architecture” is not an actionable root cause. Do not indefinitely climb toward a framework replacement or whole-system rewrite. A genuinely local defect may have a genuinely local repair.

### Make repairs converge

Before changing code, describe the invariant the repair must restore, the owner with enough knowledge and control to enforce it, and the established paths the mechanism must cover. For concurrency and lifecycle problems, also identify the actors that may mutate or publish, the intended ordering or authority semantics, the serialization domain, and the commit or invalidation point. Do not discover this design by successively patching observed pairs of actors.

Prefer a repair that contracts the problematic state space: remove unsupported sharing, unify authority, serialize at the responsible boundary, reuse an existing guarantee, or eliminate unnecessary asynchronous or duplicated coordination where the contract permits it. Additional state or coordination can be necessary, but each flag, counter, token, lock, queue, callback, or lifecycle state must enforce a distinct established guarantee with clear ownership, transitions, and validity.

If verification reveals that the repair itself introduces another failure of the same class, the repair has not restored the invariant. Replace, revert, or redesign that mechanism rather than preserving it and adding a compensating layer. Continue with multiple mechanisms only when they protect distinct established invariants; if a stable repair cannot be established within the authorized scope, report the unresolved repair constraint instead of continuing an autonomous patch chain.

### Compare sufficient alternatives

Evaluate in this order:

- **Completeness:** restore the contract, cover established same-cause paths, and preserve necessary legitimate use.
- **Responsibility:** put enforcement with the owner that has the knowledge and authority. Do not push an upper-level business policy into a generic lower-level primitive merely to centralize it.
- **Bypass resistance:** check remaining callers, writes, aliases, and publication paths. A shared helper is insufficient if normal paths still independently maintain or skip the rule.
- **Proportionality:** compare new concepts, dependencies, protocols, compatibility work, and maintenance coordination. Exclude unrelated cleanup and unsupported generality.

Deletion, inlining, merging, narrower visibility, stronger construction, ownership changes, and new boundaries are all candidates. None has a fixed preference independent of the cause.

For a range that must stay normalized, adding a check at one failing call may leave all other mutation paths unsafe. An owned operation that updates and normalizes together may be the smallest complete repair, even if multiple callers must change. Conversely, if a pure operation simply has one incorrect comparison and its boundary is sound, introducing a new state model adds no necessary guarantee.

### Define closure through behavior and structure

Behavioral acceptance covers the established trigger, prohibited effects, and relevant legitimate scenarios. Structural acceptance verifies that the responsible mechanism is corrected: bypasses are closed or intentionally constrained, repeated internal rules no longer need independent maintenance, and ownership or lifecycle aligns with the contract.

For example, moving validation into a shared function is not enough if public mutable references still bypass it. Cancelling a request is not enough if its completion can still commit invalid results. Verify the guarantee actually restored, not merely whether the original reproducer passes.

Related-path investigation must remain tied to the same mechanism. Do not make complete repair mean proving the absence of every possible defect in the subsystem.

## Mitigation, Migration, and Optional Improvement

Default to complete repair, including necessary structural changes. Do not automatically split advice into a local patch now and optional architecture later.

When urgency or migration constraints warrant mitigation, specify what it contains, what causal mechanism remains, when it can still fail, and which necessary steps close the cause. A completed mitigation is not a completed root-cause fix.

Large repairs can be incremental. Define the compatibility path and final closure condition where relevant; do not insist on an unsafe all-at-once rewrite. A complete recommendation can exceed the immediate implementation scope without authorizing changes outside the task.

Label work optional only when it is not necessary to eliminate the established cause. Do not append unrelated cleanup to each finding. If choosing a precise implementation needs more information, provide the responsible invariant and acceptance conditions instead.

## Write a Reviewable Finding

Use a concrete title describing the defect or consequence. In the body, establish conditions, mechanism, consequence, and repair direction. Include enough location and evidence for a reviewer unfamiliar with the conversation to assess it.

Treat these as information requirements, not mandatory subheadings. Follow the requested output format. Use supporting paths to establish causality rather than listing many suspicious lines with no argument.

Avoid principle-only labels, vague possible harm, arbitrary finding limits, duplicated symptoms, and architecture proposals used to conceal weak evidence. Do not downgrade a sound independent issue merely to shorten the report; remove repetition instead. The absence of a finding limit does not require continued discovery merely because more time or context remains; preserve established independent findings within the planned boundary, then stop when coverage and verification are complete.

Keep unresolved leads separate from established findings, with the specific missing fact. State actual checks and their limits, and distinguish recommendations from implemented or verified repairs. A clean review can contain no findings within its stated coverage.
