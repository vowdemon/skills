# Stability, Isolation, and Flaky Test Control

Use this file when trust in the suite is dropping because pass/fail is not reproducible.

## Load When

- A test fails intermittently.
- CI and local results diverge.
- Timing, ordering, or environment sensitivity is suspected.
- Someone proposes retries as the first fix.

## Triage Order

1. Reproduce the failure and identify whether it is product, test, or infrastructure noise.
2. Remove shared mutable state.
3. Replace fixed sleeps with condition-based synchronization.
4. Pin time, randomness, filesystem, ports, and environment inputs.
5. Hermeticize external dependencies.
6. Add retry only if infrastructure noise remains after root-cause work.

## Root-Cause Map

| Cause | Typical symptom | First correction |
| --- | --- | --- |
| Shared state between tests | Order-dependent failures | Fresh state per test |
| Timing assumption | Passes when slowed down or retried | Wait on explicit condition |
| External network or remote dependency | Random CI failures | Fake or hermetic boundary |
| Random data or time | Hard-to-reproduce failures | Inject and pin the source |
| Parallelism or thread safety | Only fails under load or CI | Remove hidden shared resources |
| Port, temp-dir, or env collisions | Machine-specific failures | Allocate isolated resources |

## Policy

- Treat flaky tests as confidence failures, not as minor annoyance.
- Keep retries as a pressure relief valve, not as the primary fix.
- Quarantine only with explicit ownership and follow-up work.
- Preserve local/CI parity wherever possible.
- Make failure mode visible enough that the next engineer can diagnose it quickly.

## Avoid

- Adding `sleep`, `wait(1000)`, or blind retry without diagnosis.
- Sharing browsers, databases, temp directories, or globals by default.
- Leaving `skip`, `xfail`, or quarantine entries to age without review.

## Source Anchors

- [pytest: Flaky tests](https://docs.pytest.org/en/stable/explanation/flaky.html)
- [Google Testing Blog: Flaky Tests at Google and How We Mitigate Them](https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html)
- [Playwright: Best Practices](https://playwright.dev/docs/best-practices)
- [Selenium: Avoid sharing state](https://www.selenium.dev/documentation/test_practices/encouraged/avoid_sharing_state/)
- [Cypress: Best Practices](https://docs.cypress.io/app/core-concepts/best-practices)
