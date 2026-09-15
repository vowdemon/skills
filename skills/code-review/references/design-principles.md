# Design Principles

Use these principles to evaluate actual semantics, responsibilities, and constraints. A surface feature such as a suffix, branch count, or forwarding method invites investigation; a demonstrated false concept, misplaced responsibility, or missing guarantee can itself establish a design problem. Do not require a runtime incident before correcting it.

Read the sections relevant to the reviewed concepts and paths. Follow their cross-references when the underlying cause crosses domains.

## Contents

- [Naming](#naming): intent, truthful vocabulary, naming contagion.
- [Responsibilities and cohesion](#responsibilities-and-cohesion): knowledge ownership, complete operations, change locality.
- [Abstractions and complexity](#abstractions-and-complexity): shared knowledge, useful boundaries, configuration, generics.
- [Control flow](#control-flow): decisions, transitions, loops, error boundaries.
- [State and data flow](#state-and-data-flow): authority, derivation, mutation, synchronization.
- [Side effects and lifecycles](#side-effects-and-lifecycles): ownership, validity, cleanup, retry.
- [API and module boundaries](#api-and-module-boundaries): public contracts, dependency direction, capabilities.
- [Comments](#comments): intent, contracts, accuracy, maintenance value.

## Naming

Names describe concepts and also become examples that later code follows. Examine both their immediate meaning and the responsibility model they encourage.

### Express intent in the usage context

Name the concept, capability, or operation meaningful to its consumer. Prefer `normalizePosition` to `calculateNormalizedSliderPositionValue` when the extra words add no distinction. Include implementation information when it is part of the relevant contract, such as an encoding or algorithm callers deliberately select.

Use the module, receiver, type, and call site to supply context. `config.load()` may be clearer than `config.loadConfig()`. Keep local names compact and public names sufficiently specific. Judge the actual import and usage context: a directory prefix can still be useful when a type is imported elsewhere. Brevity is not the goal; remove words only when meaning and disambiguation survive.

A name need not explain the entire implementation. Names, types, documentation, and module context together establish the contract.

### Keep names truthful

Check distinctions callers rely on: replace versus append, snapshot versus live view, start versus await completion, validate versus normalize, and lookup versus creation. A misleading distinction is actionable even if the implementation currently works.

Important cost and side effects must be discoverable through the API contract. Do not require every network request to carry a particular prefix or treat an ecosystem's conventional `getUser()` as misleading solely because it performs I/O.

### Require meaningful vocabulary

Generic suffixes must explain a real role or distinction. If `ProductData`, `ProductInfo`, and `ProductModel` denote different concepts, identify what differs; otherwise the names conceal the model.

`Manager`, `Service`, `Handler`, `Controller`, `Result`, `Factory`, and similar words are not forbidden. Ask what responsibility or established protocol they express. A collection of unrelated operations cannot acquire cohesion merely by being called a service. Rename to the actual concept when the responsibility is sound; reconsider the responsibility when no truthful name fits.

Use precise verbs. Remove empty wrappers such as `performSaveOperation` when `save` says the same thing. Preserve distinctions such as `handleSelectionChange` for an event handler when it does more than select an item.

### Do not name unsupported abstractions

Do not reserve vocabulary for architecture without a semantic basis: `FooManager` does not justify `FooManagerService` or `FooManagerFactory`. An approved design concept may be named before implementation; a hypothetical extension point is not established merely by naming it.

### Prevent naming contagion

Do not let an incidental identifier become an architectural rule. Each additional noun, paired role, and derived type needs an independent semantic basis.

One `useConfigLoader()` does not imply a `Loader/Saver` architecture or a family of `LoaderOptions`, `LoaderResult`, and `LoaderContext` types. Check whether loading and saving belong to one cohesive configuration responsibility before splitting them for symmetry. `config.load()/save()` may express that responsibility better; separate loaders and savers remain appropriate when they have genuinely distinct contracts and lifecycles.

Trace how foundational names propagate into public types, sibling modules, templates, and examples. If new behavior is repeatedly forced into suffixes or exceptions to preserve the vocabulary, revisit the concepts. Do not preserve a false classification by adding more names around it.

In early design, a name can already misrepresent the intended responsibility or be used to justify an unsupported split. Report that present mismatch and the proposed propagation separately from consequences that have not happened. The suffix alone does not prove future architectural decay.

Correct the source of a misleading concept and its affected uses. If only the name is wrong, rename it. If the vocabulary has encoded a wrong responsibility boundary, renaming each symptom is insufficient. See [abstraction symmetry](#do-not-build-for-symmetry).

## Responsibilities and Cohesion

A responsibility is the knowledge, rules, invariants, and reasons for change owned by a unit, not the number of technical steps it performs.

### Preserve complete concepts

Keep a concept's state, invariants, and operations together where possible. Reading, parsing, and validating may form the single responsibility of loading configuration. Conversely, parsing colors, saving recent colors, and animating a preview do not become one responsibility just because all concern color.

Judge cohesion through independent rules and axes of change. A unit that must change for unrelated presentation, persistence, analytics, and business policies may mix responsibilities. Do not mechanically split every technical concern or assume a unit can participate in only one kind of change.

### Prefer cohesion over mechanical splitting

A split should establish a real contract or isolate an independent concern. Thin validators, managers, helpers, and services that scatter one invariant can make a complete responsibility harder to maintain. Do not split by line count, number of methods, or technical steps alone.

### Assign knowledge and authority together

Place a rule with the party responsible for it and able to obtain the necessary information and control. Reading many fields from an object can reveal misplaced behavior, but does not prove every policy belongs on that data object. Cross-entity and application policies may have a different owner.

State and invariants need clear ownership. Encapsulation hides the knowledge needed to maintain valid state, not merely the fields. Identify who can mutate state, what must remain true, and what mechanism enforces it. Multiple callers can use one sound mutation boundary; multiple independent copies of the same validity rules create a different problem.

### Keep operation protocols at the responsible boundary

An operation should establish the postconditions it promises. If every caller must perform `set → normalize → refresh → sync` to preserve internal consistency, determine whether the callee should own that complete operation.

Distinguish internal maintenance from legitimate caller policy. A caller may choose when to commit a transaction or which business steps to coordinate. It should not have to rediscover hidden indexing, invalidation, or ordering rules that the owned component can maintain itself.

For a required protocol, check who owns the choice, whether the steps have meaningful public semantics, what constrains omission/reordering/repetition, and whether all callers duplicate an unvarying sequence. Prefer a complete operation, scope, construction rule, or smaller capability surface when it can enforce the invariant at reasonable cost. Do not demand elaborate state types for every conventional low-level protocol.

### Distinguish orchestration from rule ownership

A high-level flow may coordinate several components without owning their domain rules. Calling multiple responsibilities is not itself a violation of cohesion. Keep step selection and coordination at the appropriate level, while each participant maintains its own invariants. Investigate orchestration that repeatedly reaches inside participants and reproduces their policies.

### Share knowledge, not coincidental shape

Shared code should express shared semantics, constraints, and reasons for change. Similar fields or control flow are insufficient. Combining independently changing rules creates knowledge coupling even if it removes repeated lines.

When the same rule is maintained independently in several places, identify its authoritative owner and actual synchronization burden. Distinguish independent rule copies from generated code and deliberately separate policies. This is the basis for abstraction decisions in [Abstractions and complexity](#abstractions-and-complexity).

### Contain change and cross-boundary knowledge

Inspect both directions: one rule scattered across many locations, and one unit changed by unrelated rules. Good boundaries align change impact with concept boundaries.

Ask which internal facts a consumer must understand. A boundary that adds files and call hops without reducing that knowledge has not provided useful information hiding. Conversely, crossing files is not itself a defect when each contract permits local reasoning.

Use an existing maintenance operation or an established requirement to trace change propagation. Do not invent an optional future backend merely to justify isolation. Not every change can be hidden: a deliberate public-contract change legitimately affects consumers.

## Abstractions and Complexity

### Make complexity purchase a capability

Distinguish complexity necessary for domain behavior, technical constraints, reliability, compatibility, and performance from complexity introduced without corresponding benefit. An abstraction may justify itself by compressing knowledge, isolating change, enforcing constraints, limiting capabilities, or controlling lifecycle.

Evaluate the entire responsibility and its consumers. A small owner can become more sophisticated while many callers become simpler and safer. Required correctness fixes may add necessary complexity; do not reject them merely because the code becomes longer.

### Derive abstractions from established semantics

Apply [shared knowledge](#share-knowledge-not-coincidental-shape), not resemblance. A second implementation is neither necessary nor sufficient to justify an interface. A current boundary or an approved requirement may justify isolation before another implementation exists. An unsupported possibility does not.

### Remove indirection without a useful boundary

For each wrapper or layer, identify the policy, constraint, transformation, stable vocabulary, or implementation detail it isolates. A forwarding function may form a real compatibility or dependency boundary; judge its consumers and contract, not its line count.

`Controller → Service → Manager → Repository → Adapter` does not inherently improve a design. If each layer exposes the same internals and adds no guarantee, merge or remove the unnecessary indirection. If a layer genuinely shields consumers, inlining it may distribute knowledge and increase total cost.

An abstraction leaks when consumers must understand details it promises to hide. Explicit resource or flush operations can be a legitimate low-level contract; requiring ordinary business callers to manage hidden cache or scheduling internals is a different issue. Use the [operation protocol](#keep-operation-protocols-at-the-responsible-boundary) test.

### Do not mistake configuration for abstraction

Many flags, callbacks, and modes can compress unrelated operations into one entry point without giving them shared semantics. Inspect invalid combinations, branch-specific parameters, caller knowledge, and how adding a behavior affects existing modes.

For example, growing `preview/sync/legacy/retry` combinations may be an implicit state space rather than a useful general API. Prefer contracts that express the actual operations or valid combinations when that removes the ambiguity. Configuration is appropriate when its options describe a coherent capability with defined interactions.

### Make generics express relationships

Use generics and advanced types to express meaningful relationships, constraints, or reusable behavior. They can preserve units, identities, and state relationships even without several implementations. Do not introduce type machinery merely to generalize a concrete operation; compare the safety gained with the burden at definitions and call sites.

### Do not build for symmetry

A reader does not require a writer, a parser does not require a serializer, and an implementation does not automatically require a factory. Independently justify each role. Symmetry may follow from genuine bidirectional semantics; it is not evidence for them. Also inspect [naming contagion](#prevent-naming-contagion), which can initiate this structural expansion.

### Keep designs sufficient and proportionate

Compare concepts, dependencies, states, protocols, and indirection after confirming that each option satisfies the required guarantees. A small problem usually needs a small structure, but counts of conditions or implementations are not pattern-selection rules.

Recommend the smallest sufficient design. Simplicity excludes structures with no semantic value and avoids transferring necessary knowledge to scattered callers. Apply [complete root-cause repair](findings-and-remediation.md#complete-root-cause-repair) when recommending a change.

### Treat removing an abstraction as a valid choice

Deletion, inlining, merging, and narrower visibility are valid design choices when a boundary no longer returns value. So are retaining or introducing a boundary that prevents error or isolates a real constraint. Historical existence does not justify an abstraction; having only one remaining implementation does not prove it is useless.

## Control Flow

### Make decisions readable without mechanical flattening

Prefer a clear main path and local handling of boundary conditions. Early exits can reduce nested context; nesting can also express a meaningful scope or mutually exclusive structure. Judge the conditions, state, and pending actions a reader must retain together, not a nesting threshold.

Name predicates when they express stable business meaning. Avoid double negatives and inverted vocabulary that obscure the decision, but do not turn simple local expressions into unnecessary helpers. The goal is to expose the rule, not merely hide the Boolean expression elsewhere.

### Localize decisions and expose state transitions

Repeatedly distributing the same mode decision across one flow can cause coordinated edits and inconsistent cases. Rechecking a condition after relevant state changes can be necessary; verify stability before consolidating it.

When flags jointly describe phases, identify valid combinations and transitions. Check that transitions and rejected operations are apparent. Repeated branches may indicate a missing model, but neither branches nor Boolean values inherently require a state-machine framework. Use [configuration semantics](#do-not-mistake-configuration-for-abstraction) to distinguish coherent options from unrelated flows.

### Expose ordering and error boundaries

If correctness depends on omission, reordering, or repetition of steps, apply the [operation protocol](#keep-operation-protocols-at-the-responsible-boundary) rule. Do not hide required ordering in unrelated callbacks or incidental dispatch behavior.

Callbacks, events, and promises may express valid boundaries. Investigate flows whose completion, failure owner, or sequence cannot be understood from their contracts; do not demand that all asynchronous code be inlined.

Align `try/catch`, fallback, and recovery with the actual failure and transaction boundary. A broad `try` may be correct; a broad catch that conflates failures or returns false success may not be. Ordinary cases should follow the language and API's expected control-flow conventions; exception frequency alone does not determine whether exceptions are appropriate.

### Check loops and branch endpoints

Identify progress, termination, invariants, and mutations during iteration. Multiple external updates or collection mutation deserve tracing, not automatic rejection. Do not combine independent flows merely to use one loop.

Check switch fallthrough, early exits, and shared branch state. Intentional, clearly expressed fallthrough can be valid; accidental execution of the next case is not. Track required side effects and cleanup at all relevant endpoints.

When each new requirement adds flags and special branches, re-evaluate the model before recommending another condition. Minimize simultaneously interacting paths, not the number of `if` statements. For actual execution checks, use [behavioral verification](behavioral-verification.md).

## State and Data Flow

### Classify state before judging duplication

Distinguish authoritative state, derived values, caches, editable drafts, historical snapshots, and replicas or materialized views. Determine what fact each represents, when it must be consistent, and who resolves conflict or invalidity.

A single source of truth means unambiguous authority for a fact within its scope; it does not prohibit multiple representations or partitioned ownership. A cache needs a defined validity and rebuild policy. A draft may intentionally differ from committed state. Replicas require a consistency and conflict policy appropriate to their contract.

### Prefer derivation over independent synchronization

If a value follows directly from current inputs, derive it instead of maintaining another independent mutable fact. Storing `fullName` separately from `firstName/lastName` adds a consistency obligation unless it represents a different semantic value or justified materialization.

Avoid replacing a direct dependency with `A → effect → B → effect → C`. This adds scheduling, intermediate states, failure windows, and duplicate work. Effects connect external systems; they should not be the default implementation of ordinary computable relationships. For necessary materialization, identify update/invalidation ownership and failure recovery.

### Enforce invariants at mutation boundaries

Apply [knowledge and authority](#assign-knowledge-and-authority-together). Trace every relevant write path, including references returned to consumers. Centralize the responsibility for clamping, normalization, uniqueness, invalidation, and other invariants; this need not mean one giant mutation method.

Specify when the invariant must hold: throughout an operation, at a public observation point, or at transaction commit. Controlled intermediate states inside an isolated operation are not automatically invalid public states.

Multiple callers using a sound boundary are different from independent writers maintaining copies of its rules. The wider the sharing scope, the more important clear mutation authority becomes. Singletons and global stores are not inherently wrong.

### Make transformations and dependencies traceable

Preserve clear semantic stages such as `raw → parsed → normalized → validated`. New objects can make stages visible, but controlled in-place transformation is valid when aliasing, observation, and meaning remain clear.

Expose the inputs needed to understand a computation through parameters, owned state, or a well-defined context. Avoid hidden dependencies that make results impossible to explain locally; do not require all contextual information to be threaded through every function.

Make observable mutation recognizable in the contract. A query should not secretly change business state or input objects. An internal cache that preserves the promised observations is not automatically a semantic violation.

### Keep authority and propagation distinct

Prefer traceable source-to-consumer flow. Genuine two-way semantics require explicit authority, feedback termination, and conflict handling. Mutual listeners that continually repair one another often conceal an unresolved ownership problem.

Events describe occurrences; state describes a current fact. Do not use transient notifications as an implicit authoritative store without initialization, missed-event, ordering, and recovery semantics. Event sourcing is valid when the event log deliberately owns history and projections have defined replay and consistency behavior.

Data model copies need a semantic, ownership, lifecycle, compatibility, or representation boundary. Nearly identical fields do not prove models should merge; repeated mapping solely to satisfy formal layering does not prove they should remain separate.

For overlapping results and subscriptions, apply [lifecycle validity](#align-lifetimes-and-result-validity) rather than creating independent timing rules here.

## Side Effects and Lifecycles

### Make effects and their owners visible

Keep pure computation independent of environment access where practical. Concentrate external interaction at meaningful boundaries without constructing unnecessary wrappers for simple operations. Identify who owns I/O, observable mutation, registration, and expensive work through the contract; apply [truthful naming](#keep-names-truthful).

Identify a resource's creator, owner, user, and releaser. They may differ when ownership transfer is explicit. A handle discarded by one caller can still be supervised by a queue or scope; verify the real owner instead of inferring abandonment from local syntax.

### Align lifetimes and result validity

Timers, listeners, streams, file handles, sockets, workers, and asynchronous jobs need lifetimes appropriate to their owner. A resource should not silently become application-scoped when its responsibility ends with a request or session.

Distinguish whether work is still executing from whether it may still commit a result. Inputs, identity, owner destruction, and superseding work can invalidate the right to publish. Cancellation does not necessarily prevent a completion already in progress. Inspect the commit point and the actual guarantee provided by cancellation, version checks, scopes, or ownership.

Choose ordering semantics from the operation's contract. Latest-wins may suit search; independent writes may all need to complete. An older result is wrong when its authority or relevance has expired, not merely because it started earlier.

### Close resource lifecycles on relevant exits

Trace success, failure, cancellation, early return, and partial initialization. Creation/release symmetry means a closed lifecycle, not necessarily adjacent manual calls. Prefer structured cleanup such as scopes, disposables, `finally`, and RAII where available.

Verify framework ownership before demanding manual teardown. Garbage collection does not by itself prove active registrations or external work will stop. Cleanup's existence is insufficient: check that it runs for the actual instance at the correct boundary and that invalid work cannot still publish afterward.

Where semantics permit repeated teardown, make it idempotent and safe under relevant races. Swallowing every exception is not evidence of safe cleanup. Apply [operation protocols](#keep-operation-protocols-at-the-responsible-boundary) to repeated manual lifecycle sequences.

### Preserve cancellation and task supervision semantics

Distinguish expected cancellation, timeout, business refusal, and operational failure. Do not turn ordinary cancellation into misleading failure feedback or automatic retries. Logging and user notification may still be appropriate when required by the cancellation's cause and the operation's contract.

Fire-and-forget work needs an owner for failure policy, lifecycle, and completion. Work deliberately transferred to a supervisor or durable queue can outlive its initiating component. Dropping a task without such a transfer can lose error handling and validity control.

### Define effect ordering and repetition

Express correctness-relevant order in the operation or an explicit dependency. Documented language/framework scheduling guarantees may be used; accidental registration order or undocumented internals must not silently carry business correctness.

Retries need a defined policy for pacing, termination or continued operation, cancellation, resource use, and final/ongoing failure. A persistent connection service can retry indefinitely under controlled supervision; an unbounded recursive retry is a different mechanism.

Repeated side effects can arise from timeout, retry, duplicate events, and concurrent recovery. Establish idempotency, deduplication, or compensation where the business operation needs it. Identify the scope of each guarantee: a local transaction does not automatically deduplicate a remote payment. A timeout can leave the outcome unknown rather than prove no effect occurred.

Use [behavioral verification](behavioral-verification.md#partial-failure-retry-and-recovery) to trace partial completion and retry effects.

## API and Module Boundaries

### Expose a minimal but complete capability

Expose meaningful consumer operations and stable concepts, keeping implementation details private when they are not part of the intended contract. A low-level storage or collection API may legitimately expose mechanisms that would leak through a business API.

Public surface creates compatibility obligations. Do not expose capabilities for unsupported future possibilities. Keep what the supported contract actually needs.

Public types should represent the stable vocabulary consumers may depend on, not accidental internal organization. Check propagation from [naming contagion](#prevent-naming-contagion). An underlying representation can change legitimately only where the contract permits it.

### Match API granularity to usage

Do not make an interface so small that every caller must assemble an internal protocol, or so broad that one entry point absorbs unrelated contracts. Expose units of operation meaningful to consumers. Neither fragmented steps nor a broad `execute(type, options)` automatically form a good boundary; assess the knowledge and choices each requires from its caller.

### Restrict invalid states and unauthorized mutation

Use construction rules, types, visibility, and operation boundaries to make invalid use difficult. For a range invariant `min <= max`, do not indefinitely rely on every caller to check it independently when the range boundary can maintain it.

Choose constraints proportionate to risk and valid usage. Runtime enforcement at the owning boundary may be sufficient; not every invariant needs advanced compile-time encoding. External input still needs appropriate validation.

Distinguish snapshots, read-only views, and mutable capabilities. Returning a mutable internal collection is a problem when it lets callers bypass owned invariants. An intentional mutable-view contract must define and preserve its authority and lifecycle; naming a value read-only is not enforcement.

### Make arguments and results carry their actual semantics

Avoid argument ambiguity such as `save(config, true, false)` when callers cannot tell the roles apart. Named arguments, enums, or parameter objects can clarify meaning; one clear Boolean does not require a new abstraction.

Do not require ordinary consumers to choose internal cache keys, retry phases, or buffer policy that the module should own. Such choices may belong to callers of a deliberately low-level capability; determine the responsibility first.

Return enough information to distinguish outcomes callers must handle. Do not collapse conflict, cancellation, absence, and partial completion into an ambiguous success flag. A `null` or Boolean is appropriate when it completely expresses the required distinction.

Errors are part of the contract. Keep failure semantics predictable at the abstraction level, translating where necessary. Do not require every layer to use the same representation. Apply [operation protocols](#keep-operation-protocols-at-the-responsible-boundary) to required API call order and [configuration semantics](#do-not-mistake-configuration-for-abstraction) to broad mode-driven entry points.

### Align dependencies with architectural intent

Identify which policies and implementation details the project intends to isolate. Do not introduce reverse imports merely for convenience, but do not impose a universal framework-free domain layer on every project.

Separate source dependencies, runtime calls, and data/event flow. A callback from B to an interface owned by A does not by itself make A depend on B's implementation or establish a harmful cycle. Inspect contract ownership, concrete imports, initialization, and mutual internal knowledge before diagnosing circular dependency.

Respect actual public entry points. Deep imports into private modules can turn internals into an unofficial API; an explicitly exported public subpath is not a violation merely because the path is long. Reflection and direct access deserve the same contract check.

### Keep shared modules semantically owned

`shared`, `common`, `core`, and `utils` must not become relocation sites for unresolved dependencies. Several users do not by themselves establish a shared concept. Require stable shared semantics and clear ownership, rather than moving feature-specific policy into a common directory to silence an import problem.

### Preserve semantic compatibility and change isolation

Unchanged signatures do not imply compatibility. Check behavior, side effects, errors, timing, performance expectations, lifecycle, and data representation promises used by consumers.

Use [change locality](#contain-change-and-cross-boundary-knowledge) to identify implementation details that escape their boundary. A change to an intentionally public contract may require consumer updates; a storage refactor that unexpectedly changes many business callers may reveal leakage. Judge which changes the boundary actually promises to absorb.

## Comments

Comments carry contracts, intent, tradeoffs, and constraints that code may not express on its own. Evaluate their truth, usefulness, placement, and maintenance burden.

### Preserve necessary knowledge

Keep explanations of non-obvious intent, external constraints, compatibility decisions, concurrency/lifecycle guarantees, and consequences of apparently redundant code. Public API documentation can be essential. Preserve legal and copyright notices as required.

Make warnings actionable: state the condition and consequence, rather than vague advice such as having enough time. A TODO should identify a real unresolved requirement or constraint; its presence is neither automatically a finding nor proof that the issue is handled.

System-wide context may belong beside local code when it explains why that code must behave a certain way. Keep only the relevant relationship and link to maintained broader documentation where appropriate.

### Correct misinformation and remove unhelpful repetition

Flag misleading, stale, ambiguous, or poorly attached comments when they distort the contract or maintenance decisions. Remove line-by-line narration and documentation that adds no information. A documentation requirement may be legitimate; generated noise is the problem, not the existence of the requirement.

Keep history needed to explain a current constraint or rejected alternative. Routine change journals and authorship notes usually belong in version control, subject to legal or project requirements. Remove unused commented-out implementations; distinguish them from deliberate examples in documentation.

Section markers and closing-brace comments are not inherently defects and do not prove a function is too long. Assess whether they help navigation, create noise, or compensate for a demonstrable structural problem.

### Pair explanation with enforcement

Prefer code and boundaries for enforceable invariants, while retaining explanations of why those invariants exist. Comments cannot substitute for a missing owned guarantee. Conversely, deleting rationale because the code is locally readable can remove knowledge needed to modify it safely.
