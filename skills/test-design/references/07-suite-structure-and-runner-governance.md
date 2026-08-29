# Suite Structure and Runner Governance

Use this file when the question is about how the whole suite should be organized and executed.

## Load When

- Define directory layout or naming rules for tests.
- Decide markers, lanes, or slow-test scheduling.
- Align local and CI execution behavior.
- Review skip, xfail, flake, or runner-config policy.

## Governance Rules

| Concern | Default rule |
| --- | --- |
| Test layout | Keep test locations and naming conventions predictable and version-controlled |
| Runner config | Store the canonical config in the repo, not in personal shell habits |
| Fast vs slow paths | Split fast feedback from slow or environment-heavy lanes explicitly |
| Markers and categories | Declare them centrally and enforce strict validation |
| Local vs CI parity | Keep commands and environments as close as practical |
| Test code quality | Lint or review tests as seriously as production code |

## Default Execution Model

- Keep a small, high-value fast lane for routine development feedback.
- Route browser, networked, or environment-heavy suites into explicit slower lanes.
- Make it easy to run one file, one group, or one failing test locally.
- Keep the same discovery and marker rules in local and CI flows.
- Make skipped or expected-failure tests visible and reviewable.

## Review Questions

- Can a new engineer tell how to run the right subset quickly?
- Can the suite be sliced by layer, speed, or dependency profile?
- Are runner settings hidden in scripts that diverge from CI?
- Are markers or categories strict enough to prevent entropy?
- Does the suite encourage cheap feedback before expensive feedback?

## Avoid

- One giant undifferentiated test lane.
- Marker names that are free-form or undocumented.
- CI-only behavior that engineers cannot reproduce locally.
- Long-lived skip or xfail entries without ownership.

## Source Anchors

- [pytest: Good Integration Practices](https://docs.pytest.org/en/stable/explanation/goodpractices.html)
- [pytest: Flaky tests](https://docs.pytest.org/en/stable/explanation/flaky.html)
- [Google Testing Blog: Test Sizes](https://testing.googleblog.com/2010/12/test-sizes.html)
