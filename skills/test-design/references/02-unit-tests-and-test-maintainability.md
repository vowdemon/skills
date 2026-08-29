# Unit Tests and Test Maintainability

Use this file when the problem is primarily about small tests.

## Load When

- Write or review unit tests.
- Prune duplicate tests.
- Simplify brittle test setup.
- Decide whether a small test is asserting the wrong thing.

## Protect These Properties

- Immediate readability
- Single clear contract
- Observable behavior
- Minimal setup
- High diagnostic value
- Refactor tolerance

## Default Construction Rules

- Keep the structure linear: arrange, act, assert.
- Keep the semantic difference explicit in the test body.
- Use table-driven form only when all rows prove the same rule.
- Assert one core contract unless the interaction itself is the contract.
- Prefer the public API, public state change, or public side effect.
- Cover boundaries, invalid input, branches, state transitions, and past bugs before broad happy-path repetition.

## Rewrite or Delete When

- The name describes a different path than the code executes.
- The test breaks under harmless refactor while behavior is unchanged.
- The setup is larger than the semantic difference being proven.
- The same contract is already proven elsewhere with stronger or cheaper coverage.
- The helper structure hides the meaningful inputs.
- The test needs narration to explain what it is testing.

## Preferred Moves

| Situation | Preferred move |
| --- | --- |
| Many cases from the same partition | Keep one or two representative values |
| A helper hides the relevant inputs | Inline the important setup |
| One test fails for multiple unrelated reasons | Split by contract |
| An assertion depends on private state | Re-anchor the assertion to public behavior |
| Similar tests differ only in tutorial narration | Keep the strongest one and delete the rest |

## Smells

- Builders or fixtures that bury the only meaningful input difference
- Loops or branching inside the test body
- Assertions about call order with no contract guarantee
- Broad smoke tests such as "supports many types" when the semantics do not differ
- Over-DRY test helpers that make failures harder to read

## Source Anchors

- [Software Engineering at Google, Chapter 12: Unit Testing](https://abseil.io/resources/swe-book/html/ch12.html)
- [Martin Fowler: The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
