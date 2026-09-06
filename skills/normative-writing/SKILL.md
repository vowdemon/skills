---
name: normative-writing
description: Normalize settled technical rules, requirements, contracts, policies, constraints, and invariants into precise, compact, internally consistent normative text. Use when semantics are already decided; do not use to discover requirements, resolve design choices, or rewrite exploratory or historical content.
---

# Canonical Normative Writing

## Purpose

Normalize established technical rules, requirements, constraints, contracts, invariants, and system behavior into precise, compact, internally consistent normative text.

Preserve existing semantics. Do not discover requirements, resolve design questions, reconstruct rationale, or invent missing semantics.

Prioritize semantic normalization. Improve general readability only where meaning remains unchanged.

## When to Use

Apply these rules when a document or section primarily expresses decided facts or rules that require one stable interpretation.

Typical applicable documents include:

- Specifications and requirements.
- Behavioral contracts.
- Protocol and schema semantics.
- Policies, rules, and constraints.
- Domain models and invariants.
- Acceptance criteria.
- API behavioral specifications.
- Normative sections of architecture documents.
- Finalized technical definitions.

Relevant objectives include:

- Consistent interpretation by humans and AI agents.
- Mechanical validation or linting.
- Requirement traceability.
- Reliable downstream task generation.
- Comparison of revisions.
- Stable terminology across multiple documents.
- Reduction of semantic ambiguity.
- Conversion of informal requirements into normative form.

## When Not to Use

Do not apply these rules globally to documents whose purpose is exploration, explanation, preservation, persuasion, or execution decomposition.

Do not use normative writing as the primary style for:

- Brainstorming.
- Exploration documents.
- Research notes.
- Tutorials and guides.
- General README content.
- User-facing explanatory documentation.
- Narrative UX documentation.
- Raw user input.
- Conversation transcripts.
- Provenance records.
- Historical records whose original wording is significant.

Apply normative writing selectively to:

- Proposals.
- Design documents.
- Architecture documents.
- ADRs.
- Implementation plans.
- Task documents.

### Proposal Documents

Apply the rules to finalized change descriptions, scope, terminology, and impact statements.

Do not remove information merely because it is not a system rule when that information explains why the change exists.

Preserve:

- Problem statements.
- Motivation.
- Scope.
- Change intent.
- Consequences.

Do not transform tentative motivation into normative requirements.

### Design Documents

Apply the rules to finalized definitions, constraints, interfaces, invariants, and selected design behavior.

Do not collapse:

- Alternatives.
- Trade-offs.
- Rejected options.
- Rationale.
- Assumptions.
- Consequences.

Distinct alternatives may intentionally repeat the same aspects so that they remain independently comparable.

### Architecture Documents

Apply the rules strongly to sections describing:

- Components.
- Responsibilities.
- Boundaries.
- Interfaces.
- Invariants.
- Processing rules.

Apply them weakly to sections describing:

- Rationale.
- Alternatives.
- Historical context.
- Trade-offs.

### Task Documents

Do not apply rules that merge statements merely because they concern the same concept.

Tasks are optimized for execution atomicity rather than semantic cohesion.

A single specification concept may correctly produce several separate tasks.

### Provenance Documents

Do not normalize source material.

Preserve original user messages, observations, uncertainty, tentative suggestions, and historical decisions in their original semantic state.

Do not convert:

- A question into a requirement.
- A suggestion into a decision.
- An observation into an invariant.
- An uncertain statement into an established fact.

Normalization may be applied only to explicitly separated derived fields such as an AI interpretation or finalized decision, and must not modify the preserved source.

## Preconditions

Before applying the rules:

- Determine the purpose of the document or section.
- Determine whether the content is normative, explanatory, exploratory, historical, or executable.
- Apply only to normative content.
- Preserve the document's existing formal syntax unless changing that syntax is explicitly requested.
- Preserve established formats such as EARS, Gherkin, Given/When/Then, RFC keywords, schemas, tables, identifiers, headings, and machine-readable markers.
- Preserve the original semantics unless semantic correction is explicitly requested.
- Do not invent behavior needed only to make the text appear complete.
- Do not silently resolve contradictions. Surface them when they cannot be normalized without changing meaning.

## Canonical Vocabulary

Before rewriting substantial content, identify the concepts that require stable names.

For each concept:

- Select one canonical term.
- Reuse that term throughout the applicable scope.
- Replace synonyms that refer to exactly the same defined concept.
- Preserve distinct terms when they represent distinct concepts.
- Do not merge terms merely because they are linguistically similar.
- Prefer terminology already established by the document or surrounding specification.

Examples of undesirable synonym drift include using several names for the same object, state, operation, or artifact without a semantic distinction.

Canonicalization must not erase meaningful distinctions.

## Normalization Rules

- Each term is used consistently throughout the document.
- Synonyms for defined terms are not permitted.
- A canonical vocabulary is fixed and applied consistently.
- Each sentence states exactly one logical proposition, unless several variants of the same aspect are contrasted in one statement.
- When multiple statements describe the same aspect across different variants of an object, they are merged into a single contrasting statement.
- Statements about the same concept are merged and never split artificially.
- Distinct aspects of a concept are merged only when neither describes an independent property.
- Every sentence contributes to the logic being described.
- Information without effect on the normative logic is removed.
- Redundancy for emphasis and repetition without new information are avoided.
- The order of sentences forms a traceable cause-and-effect chain.
- The structure follows the logical processing steps.
- Wording is precise, technical, and compact, in the register of legal or normative technical texts.
- Where meaning is unchanged, the shortest wording is used.
- Sentences are bounded and readily understood.
- Colloquialisms, value judgments, and figures of speech are not permitted.
- Figurative, metaphorical, or ambiguous wording is not permitted where a more precise statement is possible.
- Similar statements may vary in wording only when the variation does not introduce terminology inconsistency or semantic ambiguity.
- Recurring linguistic patterns without a technical purpose are avoided.
- The text is divided into labeled sections with headings.
- A section is formed only when it improves orientation.
- Content is written exclusively as bullet points when the surrounding document format permits it.
- A bullet holds one or more related sentences.
- A bullet groups the sentences that define the same property or rule of an object into a micro-narrative.
- A new bullet begins at a change of object, property, or processing step.
- A bullet holds at most three sentences unless the surrounding formal syntax requires another structure.
- Enumerations of more than three terms are given in parentheses rather than embedded in running prose when doing so improves compactness.
- Temporal properties of an object (start, end, validity, overlap) form one concept and are merged.

## Semantic Cohesion

Prefer grouping by semantic property rather than by sentence length.

For example, if several statements jointly define what an object persists, they should normally be expressed together rather than distributed across unrelated paragraphs.

Do not merge properties that can vary independently.

The following are normally independent and should remain separately expressible:

- Identity.
- Stored data.
- Ordering.
- Lifecycle.
- Failure behavior.
- Permissions.
- Visibility.
- Temporal validity.
- Side effects.

The exact decomposition depends on the domain.

## Contrast

When multiple variants differ only in the value or behavior of the same property, prefer one contrasting statement over several repetitive statements.

Do not use this rule when the variants are being evaluated as independent design alternatives.

Contrast is appropriate for normative differentiation.

Contrast is not a replacement for design comparison.

## Causality and Processing Order

Arrange statements so that a reader can trace:

1. Preconditions.
2. Trigger.
3. Processing.
4. State change.
5. Result.
6. Failure or exceptional behavior.

Use this order only when those stages exist.

Do not manufacture causal relationships between independent rules.

## Redundancy

Remove repetition when repeated text contributes no new semantics.

Preserve repetition when required by:

- A formal grammar.
- Independent acceptance scenarios.
- Independently testable branches.
- Self-contained alternatives.
- Traceability requirements.
- Deliberate task decomposition.

Semantic redundancy and structural repetition are not equivalent.

## Normative Strength

Preserve the original strength of statements.

Do not change:

- Optional behavior into required behavior.
- Suggested behavior into required behavior.
- Current behavior into guaranteed behavior.
- Implementation detail into public contract.
- Example behavior into universal behavior.

When normative keywords are already used, preserve their intended strength.

Examples include:

- MUST.
- MUST NOT.
- SHALL.
- SHALL NOT.
- SHOULD.
- MAY.

Do not introduce normative keywords merely to make prose appear more formal unless the document already uses a normative convention or the user explicitly requests one.

## Formal Structures

When the source uses a formal structure, optimize inside that structure rather than replacing it.

### Gherkin

Preserve:

- GIVEN.
- WHEN.
- THEN.
- AND.
- BUT.

Each scenario should continue to represent one coherent observable behavior.

Do not merge scenarios solely to remove textual repetition when they represent independently testable behavior.

### EARS

Preserve the distinction between:

- Ubiquitous requirements.
- Event-driven requirements.
- State-driven requirements.
- Unwanted-behavior requirements.
- Optional-feature requirements.
- Complex requirements.

Do not rewrite an EARS condition into unconditional prose.

### Structured Specifications

Preserve:

- Requirement identifiers.
- Cross-references.
- Machine-readable markers.
- Schema names.
- Field names.
- API names.
- File paths.
- Code identifiers.

Canonical vocabulary rules apply to prose terminology, not to arbitrary renaming of established technical identifiers.

## Handling Ambiguity

When two statements appear redundant, first determine whether they are actually equivalent.

Do not merge them if they differ in:

- Scope.
- Trigger.
- Subject.
- Object.
- Time.
- State.
- Failure behavior.
- Normative strength.
- Observable result.

If equivalence cannot be established without inference, preserve both statements or flag the ambiguity.

## Handling Contradictions

Do not silently choose one conflicting statement.

When two statements cannot both be true:

- Preserve the conflicting semantics.
- Identify the conflict.
- State which terms or conditions conflict.
- Request resolution only when resolution is necessary to complete the requested work.

If the task permits partial output, normalize all non-conflicting content first.

## Information Preservation

Compression must not reduce semantic coverage.

After rewriting, every normative fact in the source must be:

- Preserved explicitly.
- Preserved by an equivalent combined statement.
- Removed only because it is demonstrably redundant.
- Flagged if its meaning cannot be preserved safely.

The shorter version is not better when it carries less information.

## Output Quality Check

Before finalizing the rewritten document, verify:

- One concept has one canonical term.
- No synonym drift remains for defined terms.
- No normative rule has been lost.
- No new rule has been invented.
- No normative strength has changed.
- Independent properties remain distinguishable.
- Related properties are not artificially fragmented.
- Genuine alternatives have not been collapsed.
- Cause and effect can be traced.
- Processing order is coherent.
- Temporal rules are internally grouped.
- Redundant statements have been removed where safe.
- Formal syntax remains valid.
- The resulting text is shorter or more structured only where meaning remains unchanged.

## Operating Principle

Treat document types as different information states.

A typical flow is:

`provenance → exploration → design → proposal → specification → tasks`

These stages have different purposes:

- Provenance preserves information.
- Exploration expands possibilities.
- Design compares and resolves possibilities.
- Proposal communicates the intended change.
- Specification normalizes the resulting semantics.
- Tasks decompose those semantics into executable work.

Apply the strictest normalization near the specification stage.

Do not force earlier or later stages to adopt specification-style compression when doing so would destroy information required by their purpose.

## Core Rule

Apply canonical normative writing only to information whose meaning has already converged.

Preserve uncertainty where uncertainty is meaningful, preserve alternatives where comparison is meaningful, preserve history where provenance is meaningful, and preserve decomposition where execution is meaningful.
