# UI Automation and User-Visible Behavior

Use this file for browser, UI, or interaction-heavy tests.

## Load When

- Choose selectors or locator strategy.
- Decide what a UI test should assert.
- Review browser tests that feel brittle.
- Replace click-heavy setup with a more stable flow.

## Locator Priority

| Priority | Prefer | Use when |
| --- | --- | --- |
| 1 | Role + accessible name | The UI exposes stable semantics to users |
| 2 | Label, placeholder, visible text | The text is part of the product contract |
| 3 | Stable test-only attribute such as `data-testid` or `data-cy` | Semantics are insufficient or text changes too often |
| 4 | Structural selectors | Only as a last resort and only if stable by contract |

## Default Rules

- Assert what the user can observe, not internal component state.
- Prepare authentication, fixtures, and setup programmatically whenever possible.
- Keep each test isolated; do not reuse browser state as hidden coupling.
- Use condition-based waiting and built-in assertions; do not rely on fixed sleeps.
- Replace third-party systems with controlled boundaries unless the third party is itself under test.

## Preferred Workflow

1. Create or seed state directly.
2. Navigate to the smallest page or screen that proves the contract.
3. Interact through user-visible affordances.
4. Assert the user-visible outcome.
5. Keep setup and assertions separate enough that failures are diagnosable.

## Avoid

- CSS-path, nth-child, or DOM-shape selectors without a contract guarantee.
- Tests that spend most of their runtime reaching the real starting state.
- Assertions on implementation-only classes or internal state snapshots.
- Fixed waits added only to hide synchronization problems.

## Source Anchors

- [Testing Library: Guiding Principles](https://testing-library.com/docs/guiding-principles)
- [Playwright: Best Practices](https://playwright.dev/docs/best-practices)
- [Cypress: Best Practices](https://docs.cypress.io/app/core-concepts/best-practices)
- [Selenium: Avoid sharing state](https://www.selenium.dev/documentation/test_practices/encouraged/avoid_sharing_state/)
