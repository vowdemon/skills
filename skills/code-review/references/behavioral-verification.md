# Behavioral Verification

Use the relevant checks to test the system's actual contracts. Select scenarios from the reviewed operation and its risks; do not mechanically apply every case to every function.

## Contents

- [Establish the behavioral contract](#establish-the-behavioral-contract)
- [Inputs, decisions, and state transitions](#inputs-decisions-and-state-transitions)
- [Concurrency and result authority](#concurrency-and-result-authority)
- [Partial failure, retry, and recovery](#partial-failure-retry-and-recovery)
- [Resources and exit paths](#resources-and-exit-paths)
- [Compatibility and regression](#compatibility-and-regression)
- [Choose discriminating verification](#choose-discriminating-verification)

## Establish the Behavioral Contract

For a relevant operation, identify its entry, inputs and identity, initial state, legal preconditions, decisions, state updates, external effects, commit/completion point, and outcome exposed to callers.

Check both required results and prohibited effects. A correct return value does not prove the whole operation is correct if it also overwrites another user's state, duplicates a write, or leaves the owner unusable.

Use requirements, supported callers, and effective API contracts to establish expectations. Distinguish a caller violating a documented low-level precondition from a boundary failing to enforce a constraint it owns. Existing tests can document intent but are not automatically correct.

For a design without runnable code, walk concrete scenarios through the proposed responsibilities, state model, and operations. Identify where each required decision, failure, and recovery would be represented. An unspecified detail is a question unless the proposal's commitments already make the requirement impossible or contradict a necessary constraint. Do not invent missing implementation as evidence of a bug.

## Inputs, Decisions, and State Transitions

Choose relevant values: absent, empty, malformed, boundary, duplicate, conflicting, and legitimately unusual inputs. Trace authorization and validation through the actual entry rather than assuming a helper is publicly reachable.

Identify the invariant and when it must hold. Check transitions from valid states, rejection of forbidden transitions, and whether success/failure markers describe the real completion state. Controlled intermediate inconsistency can be valid inside an isolated transaction; determine whether consumers can observe it.

Include lifecycle changes that affect the operation: first use, repeated invocation, re-entry, reload, account or owner change, and restored persisted state. Verify that failure does not leave a permanent busy flag, stale authority, unusable object, or misleading success state.

For branching and loops, check boundary conditions, progress and termination, side effects during iteration, branch endpoints, and state changed between repeated guards. A guard before a mutation or asynchronous wait may not remain sufficient afterward.

Use the [control-flow principles](design-principles.md#control-flow) to distinguish an execution defect from a representation that makes future modifications unnecessarily fragile.

## Concurrency and Result Authority

Determine the required semantics first: latest result wins, all independent operations complete, exclusive ownership, ordered processing, or another explicit policy. Do not impose latest-wins on independent writes.

Construct a concrete interleaving when concurrency matters. For example:

1. Operation A reads or captures a valid context.
2. The identity, inputs, or owner changes, or operation B commits.
3. A resumes and attempts to publish.

At the publication point, what grants A the right to commit? Verify what an abort signal, generation token, lock, transaction, or structured scope actually guarantees. Cancelling execution and invalidating publication are distinct; completion may already be queued, cancellation may be cooperative, or the external effect may already have happened.

Check state and cache keys against the correct identity and scope. Clearing visible state on logout is insufficient if old work can repopulate it. Conversely, work explicitly owned by an independent background service may correctly survive the initiating view.

For shared mutation, check atomicity of read-modify-write operations, competing ownership, lost updates, and whether the mechanism covers all writers. A local lock does not necessarily serialize another process or external writer.

When evaluating or verifying a repair, identify the complete synchronization or authority domain: the relevant actors, the state or result they may mutate or publish, and the point that orders or validates the commit. A mechanism that handles only the observed pairwise interleaving is incomplete when another established actor or lifecycle transition remains outside that domain.

A counter, generation token, lock, queue, cancellation check, or similar mechanism is not proof by its presence. Verify its owner, update or exclusion rule, validity and cleanup, commit check, and coverage of the relevant actors. If the mechanism creates another same-class interleaving, treat that as evidence that the repair strategy is unsound rather than adding another compensating mechanism by default.

Investigate practical reachability and existing protection before reporting an interleaving. Explain the ordering assumption if it remains unresolved.

## Partial Failure, Retry, and Recovery

Identify which business result must be atomic and which implementation steps have independent effects. Locate the first irreversible effect and the point after which a caller can no longer safely assume nothing happened.

For each material failure boundary, determine:

- Which earlier effects have already succeeded?
- What state is visible, persisted, or reported?
- Does the caller know whether the result is failed, partial, or unknown?
- Can the operation retry safely, and which effects would repeat?
- Who owns rollback, compensation, deduplication, or later recovery?
- Can cleanup or recovery itself fail, and what remains then?

A timeout can mean the outcome is unknown. Do not infer that a remote operation failed merely because no response arrived. Check whether retry identifiers remain stable for the same logical operation, whether distinct operations stay distinct, and how long and where deduplication applies.

A transaction protects only its actual participants and boundary. It does not automatically include a remote payment, email, or queue submission. Likewise, a retry loop with a limit does not make a non-idempotent operation safe.

Verify pacing, cancellation, resource use, and eventual or ongoing failure policy. Persistent supervised reconnect can legitimately continue; immediate recursion or accumulating retry tasks may not.

Distinguish expected cancellation, business refusal, and operational errors where callers need different actions. Check catches that swallow the difference, fallbacks that manufacture success, and error handling that unintentionally repeats effects.

After recovery, verify not only that the visible symptom disappears but that the operation can be used again without stale flags, duplicate resources, or inconsistent persisted state.

## Resources and Exit Paths

Apply [lifecycle ownership](design-principles.md#side-effects-and-lifecycles) to the actual resource instance. Identify acquisition, transfer, validity, and release, then examine success, error, cancellation, early return, and partial acquisition.

Check framework-provided disposal before requiring manual calls. If ownership is automatic, verify its scope and applicability to this resource. A cleanup method that is never reached for the instance does not close its lifecycle.

Trace what happens during teardown: can callbacks still run, tasks still commit, or a second release race with the first? If repeat release is permitted, verify idempotence without masking real failures. If it is prohibited, verify the ownership mechanism prevents it.

Fire-and-forget is not established by syntax alone. Look for the supervisor, durable handoff, or failure policy; report unowned work when these guarantees are actually absent.

## Compatibility and Regression

For a change, compare old and new behavior against supported uses. Check the semantics consumers rely on, including errors, side effects, ordering, cancellation, default values, performance expectations, and lifecycle. A signature that compiles may still break its contract.

For persisted data or protocols, inspect readers and writers that coexist during rollout, supported older data, version handling, migration failure, and restart recovery where relevant. Do not assume every deployment requires mixed-version support; establish the applicable policy.

For proposed simplification or deduplication, check whether formerly independent concepts have different behavior or change requirements. Passing one shared-path test cannot establish that merging them preserves both contracts.

For performance or resource claims, connect workload, operation frequency, scale, and cost to an actual requirement or concrete degradation. Do not turn asymptotic notation or a cache's existence into a priority claim without context.

## Choose Discriminating Verification

Use the least costly check that resolves a material uncertainty. Useful checks include a failing transition, controlled concurrent ordering, failure after the first committed effect, duplicate delivery, owner invalidation before completion, and a supported old caller or data format.

A test should distinguish the suspected mechanism from correct behavior. Prefer assertions on outputs, prohibited side effects, state validity, and repeatability over assertions that merely duplicate implementation structure. For structural repairs, also inspect or exercise remaining bypass paths and same-cause uses.

Separate what is statically established, dynamically observed, assumed, and still unknown. A successful test supports its covered scenario, not every schedule or input. A failing test may expose an environment or harness problem; investigate before attributing it to the product.

Do not require a test run for a complete static proof. Do not treat lack of reproduction as disproof when the test never exercised the required condition. State the missing condition and the next evidence that would resolve it.

Use [Findings and Remediation](findings-and-remediation.md) to decide whether the remaining uncertainty affects existence, impact, or only the repair choice.
