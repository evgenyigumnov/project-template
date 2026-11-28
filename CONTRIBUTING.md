# Development Guidelines

This document defines the general workflow for making changes. Detailed conventions for code, testing, deployment, and security are described in `CONVENTIONS.md`, `TESTING.md`, `DEPLOYMENT.md`, and `SECURITY.md`.

`code-reviewer` owns the PR and code review rules.

## Preparing Changes

- Before starting, review the task, related requirements, and relevant documents. Create the task folder and `TASK.md` before delegating or editing, following `AGENTS.md` and `tickets/README.md`.
- Keep changes within the scope of the task and the agreed areas of responsibility.
- For a large task, define sequential stages and pass each stage's result to the next person responsible.
- If an API, data schema, security, or infrastructure is affected, involve the corresponding owner.
- For every task that changes application code or behavior, involve `technical-writer` as a required stage. For tasks involving contracts, operations, or user workflows, also assess the impact on instructions and help materials. The writer receives confirmed requirements and implementation facts, updates the relevant materials, or records in the report why no changes are needed. For a documentation-only task, the writer prepares the text as the responsible author.
- For a new or substantially changed visible screen, component, or user workflow, prepare a UX prototype before frontend development and perform the required UI/E2E checks after implementation, as described in `AGENTS.md`.
- For a new application or a change to operational expectations, specify the target profile in the task: local prototype/MVP or production candidate/production. Set requirements in proportion to the profile; do not claim production readiness without checks in the target environment.

## Checks

After implementation, hand every new or changed behavior to `unit-tester`: they write or update unit tests, check positive, negative, and edge-case scenarios, and run the full applicable unit test suite. Frontend and backend developers provide exact production code paths and expected scenarios, but do not add new unit tests.

`technical-writer` reviews the instructions and help materials affected by the change and updates them after the relevant owners confirm the behavior. The task must include a `TECHNICAL_WRITING_REPORT.md` listing the updated paths or explaining why no changes were needed.

If `unit-tester` finds that an existing unit test is no longer compatible with the agreed change, the relevant developer may make a minimal adjustment to that existing test only. They do not add new test cases or remove checks just to make the suite pass. `unit-tester` reviews the adjustment and reruns the full suite. If a test run reveals a defect in production code, the relevant developer fixes it, then `unit-tester` verifies the regression coverage and reruns the tests.

UI/E2E tests from `ui-tester` are required for any change to visible behavior. Assign API/integration and other specialized checks based on the changed contract and risks. `TESTING.md` describes the full process and reporting requirements.

Before completing the task, include the applicable `UNIT_TEST_REPORT.md` and `UI_TEST_REPORT.md` in the task folder. Reports must contain the exact commands and actual results. If a required check cannot be run, record the reason and the scenarios that remain unchecked; the task is still incomplete, and the check is failed or blocked.

## Pull Requests and Code Review

In the PR description, link to the task folder and state the purpose of the change, the main areas affected, checks performed, risks, and any required migration or operational actions. Confirm that the applicable test reports are in the task folder and that all required checks passed; otherwise, state the blocker and do not present the task as complete. The reviewer checks correctness, regressions, security, consistency with the architecture, and adequacy of the checks. Review comments must include the path, context, and expected fix.

## Commits

The repository does not yet define a commit message format. Do not introduce a new mandatory format without agreement. Do not create or push commits unless the user explicitly asks.