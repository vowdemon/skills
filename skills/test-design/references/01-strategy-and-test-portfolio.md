# Strategy and Test Portfolio

Use this file to choose the right layer and the right overall mix.

## Load When

- Decide whether to add, move, or delete coverage.
- Debate unit vs integration vs E2E placement.
- Discuss coverage targets, test counts, or confidence strategy.
- Notice an `ice cream cone` or `hourglass` suite shape.

## Answer These Questions

1. What unique risk needs coverage?
2. What is the cheapest layer that can prove that risk?
3. Is the risk pure logic, boundary collaboration, protocol drift, or end-user completion?
4. Is this request really a missing middle-layer problem disguised as an E2E request?
5. Would another test add new protection or only repeat an existing conclusion?

## Default Portfolio Rules

- Keep most tests small, deterministic, and in-process.
- Use integration tests to cover adapters, wiring, persistence, serialization, and module collaboration.
- Keep large or end-to-end tests rare and tied to release-critical journeys.
- Add a higher-layer test only when a lower layer cannot prove the same risk.
- Treat coverage percentage as a lagging indicator, not as the goal.
- Prefer repairing the portfolio shape over adding one more large test.

## Layer Selection

| Risk | Default layer | Promote higher only if... |
| --- | --- | --- |
| Pure logic, branching, state transitions | Unit | Framework wiring or cross-boundary behavior materially changes the outcome |
| Repository, adapter, serializer, configuration wiring | Integration | A real user-visible cross-system flow is the risk |
| Schema or protocol drift between systems | Contract or integration | Only the full deployed chain can prove the contract |
| Release-critical user journey | End-to-end | The journey is already fully protected below and UI adds no confidence |
| Browser interaction semantics | UI or component integration | Real browser behavior is irrelevant to the contract |

## Smells and Corrections

| Smell | Correction |
| --- | --- |
| Large tests are growing because lower layers feel inconvenient | Add or repair integration tests instead |
| UI tests are proving simple domain rules | Move those rules down to unit or integration tests |
| Coverage discussion has no risk discussion | Reframe around failure cost and missing contracts |
| Many E2E variants differ only by data | Collapse to one path unless the variant adds distinct business risk |
| Few or no middle-layer tests exist | Fix the hourglass before adding more top-layer coverage |

## Escalate Carefully

- Allow more high-level coverage for safety-critical, compliance-heavy, or migration-heavy systems.
- Keep the same default question: what unique risk does the higher layer prove?
- Do not invent quotas. Use the portfolio rules as direction, not as a rigid formula.

## Source Anchors

- [Software Engineering at Google, Chapter 11: Testing Overview](https://abseil.io/resources/swe-book/html/ch11.html)
- [Martin Fowler: The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [Google Testing Blog: Fixing the Test Hourglass](https://testing.googleblog.com/2020/11/fixing-test-hourglass.html)
- [Google Testing Blog: Test Sizes](https://testing.googleblog.com/2010/12/test-sizes.html)
