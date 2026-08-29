# Test Doubles, Hermeticity, and Dependencies

Use this file to control dependencies instead of letting dependencies control the tests.

## Load When

- Choose between real collaborator, fake, stub, or mock.
- Decide how to isolate network, filesystem, clock, randomness, or third-party APIs.
- Diagnose tests that are slow, flaky, or hard to reason about because of external boundaries.

## Decision Order

1. Use the real collaborator if it is in-process, fast, deterministic, and easy to set up.
2. Use a fake or stub when the boundary is slow, external, expensive, or hard to control.
3. Use a mock only when the contract being tested is the interaction itself.
4. Make larger tests hermetic before adding retries or more waiting.

## Default Boundary Handling

| Dependency kind | Default handling | Notes |
| --- | --- | --- |
| Pure domain collaborator | Real implementation | Prefer reality when it is cheap and deterministic |
| Clock, randomness, UUIDs | Inject and pin | Do not depend on wall-clock luck |
| Network or third-party API | Fake, stub, or local hermetic server | Avoid live remote systems in routine suites |
| Database or repository boundary | Real local instance or high-fidelity fake | Choose the cheapest option that still proves the adapter contract |
| Message bus, queue, event broker | Controlled fake or hermetic local environment | Keep ordering and delivery deterministic |
| Browser-side external service | Mock at service boundary | Do not let unrelated third parties decide pass/fail |

## Default Rules

- Replace the boundary, not the core logic under test.
- Prefer fakes over interaction-heavy mocks when behavior matters more than call choreography.
- Keep fake behavior aligned with the real contract or the fake becomes a liability.
- Give shared fakes ownership and consistency checks when they matter broadly.
- Treat hermeticity as a design target, not as a later cleanup task.

## Avoid

- Verifying internal call trivia with mocks.
- Allowing tests to touch live shared environments by default.
- Building production abstractions only to make mocking possible.
- Keeping a fake that silently drifted from the real protocol.

## Escalate Carefully

- Allow real external systems only when the external boundary is the thing being validated.
- If that happens, isolate those tests into explicit lanes and keep them out of fast feedback paths.

## Source Anchors

- [Software Engineering at Google, Chapter 13: Test Doubles](https://abseil.io/resources/swe-book/html/ch13.html)
- [Software Engineering at Google, Chapter 14: Larger Testing](https://abseil.io/resources/swe-book/html/ch14.html)
- [Selenium: Mock external services](https://www.selenium.dev/documentation/test_practices/encouraged/mock_external_services/)
