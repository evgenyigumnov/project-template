# Testing Strategy

`unit-tester` is responsible for creating and maintaining unit tests, checking coverage completeness, and running the full applicable suite. Developers are responsible for production code and provide the tester with changed paths, expected behavior, and important scenarios. The UI/E2E section is maintained by `ui-tester`. Testing is a required part of completing any task that changes application behavior.

## Required Rules

- For every behavior change, `unit-tester` creates or extends automated unit tests for the changed logic. This applies to frontend, backend, API, and bug-fix work. Developers do not create new unit tests as part of their implementation.
- After implementation, the developer gives `unit-tester` the list of changed paths, expected behavior, positive, negative, and boundary scenarios, as well as the test entry point and available test commands. If the developer finds an issue or testing limitation during local verification, they report it to the tester and the orchestrator.
- A developer may change an existing unit test only in agreement with `unit-tester`, when it has been confirmed that the test became incompatible with an agreed contract change (for example, an allowed format or public behavior changed). Such a change is limited to keeping the existing check current; it does not replace tests for new or changed logic. `unit-tester` reviews the change and reruns the suite. If a test reveals a defect in production code, the developer fixes the production code instead of changing the expected behavior in the test.
- For any change to visible behavior, a screen, an action, a state, or a user flow, UI/E2E tests are also created or updated. `ui-tester` writes and runs them after the change is implemented.
- Tests are added as part of the same task, not deferred to a separate future task. The orchestrator assigns `unit-tester` to every task that changes behavior and `ui-tester` to every task that changes visible behavior or a user flow.
- If the project does not yet have the relevant test runner or test command, this is not a reason to skip tests. `unit-tester` or `ui-tester` describes the smallest suitable setup and passes it to the orchestrator; the orchestrator assigns it to the configuration owner and resumes testing after setup.
- Separate tests by level: unit tests check isolated logic, integration/API tests check interactions between components, and UI/E2E tests check user actions and visible states. One level does not replace another when a task affects both.
- Check positive, negative, and boundary cases; add a regression check for bug fixes.
- Do not weaken or remove checks to make a run pass.

## Verification Levels

- **Unit** — behavior of isolated functions and modules; required for every behavior change. `unit-tester` creates and updates tests, checks positive, negative, boundary, and regression scenarios, and runs the full suite. The developer for the changed stack communicates behavioral requirements and helps fix any production-code defect that is found; they are not responsible for authoring new unit coverage.
- **Integration/API** — interactions between components and API contracts; required when a task changes these interactions. The orchestrator assigns an owner for the checks based on the affected stack. This is a separate level and does not transfer responsibility for new unit tests from `unit-tester` to the developer.
- **UI/E2E** — visible screens, actions, states, errors, and user flows; required for any behavior change visible to the user. The owner is `ui-tester`.
- Additional checks, such as static security analysis, are assigned according to the task's risk and the availability of suitable tools.

## Tools and Commands

- Before choosing a command, study the actual application structure, test configuration, and available scripts. Do not assume directory, runner, or command names.
- If a test runner is already configured, use it. If there is none, first prepare a specific minimal setup plan for the actual stack and pass it to the orchestrator; tests cannot be considered complete until setup and a test run have taken place.
- `unit-tester` runs the project's full applicable unit-test suite, not just new or affected tests. In the report, provide the exact commands, final exit code, warnings, and environment limitations.
- Project commands and coverage requirements have not yet been defined. Record confirmed commands and thresholds in this document when the relevant configuration is added; do not invent a universal coverage percentage. Evaluate completeness against the requirements and risks of the specific logic, not just the number of tests or overall percentage.

## Required Report and Completion Criteria

For a task that changes behavior, `unit-tester` provides the orchestrator with a report in `tickets/<ID>-<slug>/UNIT_TEST_REPORT.md`. For a task with UI/E2E checks, `ui-tester` provides a report in `tickets/<ID>-<slug>/UI_TEST_REPORT.md`. If the project uses another agreed location for its ticket folder, use that location.

Each report contains:

- scenarios checked and test files created or changed;
- exact commands and their actual results;
- warnings and environment limitations;
- uncovered scenarios, defects, and blockers; explicitly state if there are no gaps.

The orchestrator does not close a task as completed and verified until all applicable reports have been received and the tests have passed. If tests cannot be created or run, `unit-tester` records the specific reason, uncovered scenarios, and owner of the next step; the verification status remains blocked or failed, not successful.