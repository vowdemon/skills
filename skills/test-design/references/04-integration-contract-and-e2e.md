# Integration, Contract, and End-to-End Tests

Use this file when the risk is about real collaboration, real boundaries, or real user completion.

## Load When

- Choose between integration, contract, and end-to-end coverage.
- Decide whether a unit-level rule should be promoted upward.
- Evaluate whether the suite is missing middle-layer tests.
- Scope large tests that are becoming too broad or too costly.

## Default Promotion Rules

- Stop at integration when the main risk is adapter behavior, wiring, serialization, persistence, or module collaboration.
- Use contract tests when independently evolving systems can drift at their shared interface.
- Use end-to-end only when business completion across the real chain is the risk.
- Do not promote a rule upward just because the lower-level test feels less glamorous.

## Test-Type Selector

| Situation | Default test type | Notes |
| --- | --- | --- |
| Repository, database mapping, serializer, adapter | Integration | Prove the boundary directly |
| Service-to-service schema or API contract | Contract or integration | Keep it close to the interface |
| Critical checkout, signup, auth, payment, or publish flow | End-to-end | Keep one strong mainline before adding variants |
| UI is not part of the confidence argument | API or service-level end-to-end | Do not force browsers into the design |
| Failure requires too much manual diagnosis | Add or improve integration tests below | A diagnosability gap usually points downward |

## End-to-End Scope Rules

- Tie each E2E test to a release-critical journey or cross-system contract.
- Keep the assertion focused on business completion, not internal implementation.
- Keep variants only when they add distinct business risk.
- Keep data setup as direct and programmatic as possible.
- Keep page objects limited to structure and interaction helpers; do not bury business assertions inside them.

## Avoid

- Using E2E to compensate for missing integration coverage.
- Checking simple domain logic only through UI flows.
- Letting one E2E test assert many unrelated contracts.
- Building a page-object layer that becomes a second test framework.

## Source Anchors

- [Software Engineering at Google, Chapter 14: Larger Testing](https://abseil.io/resources/swe-book/html/ch14.html)
- [Martin Fowler: The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [Google Testing Blog: Fixing the Test Hourglass](https://testing.googleblog.com/2020/11/fixing-test-hourglass.html)
- [Selenium: Page object models](https://www.selenium.dev/documentation/test_practices/encouraged/page_object_models/)
