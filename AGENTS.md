# Task Workflow and Pi Skills

The main Pi session (the orchestrator) is responsible for defining the task, selecting specialists, handing off context, and checking that the result is complete. Before implementation, create a task folder according to `tickets/README.md` and put a `TASK.md` in it containing the user's original request. When the rules below require role assignments or a task has multiple stages, use the `.pi/skills/subtask-orchestrator/SKILL.md` skill: it launches a separate one-shot Pi process for each assigned skill, passes along context, and receives a structured report. Do not delegate or edit files until the task context has been saved.

## Required Triggers and Stages

1. For every task that changes application code or behavior, assign suitable developers, `unit-tester`, and `technical-writer`. The orchestrator creates one plan step for each role; the runner passes the skill name to the child Pi process through `--skill` and requires it to load the role instructions before starting work. The developer implements production code and gives the tester the affected paths and scenarios. `unit-tester` creates and updates unit tests for new and changed logic, checks coverage completeness, and runs the full applicable suite. `technical-writer` assesses the impact on instructions, help, and guides; after behavior stabilizes, they update the relevant documentation or record why no changes are needed. Also assign `technical-writer` for tasks changing APIs/contracts, data, security, infrastructure, operations, or user scenarios. For tasks only creating or changing documentation, the technical writer authors the result. A developer may make a targeted adjustment only to an existing test if `unit-tester` finds that it became incompatible with the agreed change; the tester reviews that adjustment and reruns the suite.
2. For feature tasks that add capabilities or significantly change functionality, and for creating a new application or project, assign the `planner` role after saving `TASK.md` and before handing implementation to developers. Launch it as a separate Pi worker through the orchestrator skill. The planner studies the actual codebase and applicable documents, or explicitly records that no application/source code exists, then creates a staged plan in `tickets/<ID>-<slug>/PLAN.md`. The plan contains the sequence of changes, known paths or proposed areas, responsible roles, dependencies, and completion criteria. A planner is usually unnecessary for a routine bug fix; the orchestrator may assign one if the fix is complex, spans modules, or requires several dependent stages.
3. Stage implementers report actual changes, exact paths, and verification results. After each stage, the orchestrator passes those results to the assigned `planner` and asks them to update statuses, completed work, deviations, and blockers in `PLAN.md` before proceeding to the next stage. The orchestrator checks that the plan is current when moving between stages and before closing the ticket. Mark a stage complete only when the implementer has confirmed its result; do not record checks that were not run as successful.
4. For a new or substantially changed visible screen, component, or user scenario, assign `ux-ui-expert` before frontend development unless there is a current, approved prototype. Store the result in `prototypes/<ID>-<slug>/`; pass its path to the frontend developer and `ui-tester`. A prototype is not needed for tasks with no visible UI.
5. For any change to visible behavior or a user flow, assign `ui-tester` after implementation. They create or update UI/E2E tests and run the full applicable suite.
6. For changes to APIs, data schemas, security, or infrastructure, assign owners for those areas. Respect dependencies: do analysis, UX, and contract alignment first, then implementation; testers receive the completed changed paths and scenarios after implementation. The workflow runner executes steps sequentially; declare dependencies so results from earlier roles are passed along, and do not assume parallel execution.
7. Before completion, check that specialists have returned reports and artifact paths. Developers list affected scenarios, production-code paths, and build/static-check results; `unit-tester` confirms unit tests and their full run in `tickets/<ID>-<slug>/UNIT_TEST_REPORT.md`, `ui-tester` confirms UI/E2E checks in `tickets/<ID>-<slug>/UI_TEST_REPORT.md`, and `technical-writer` records the documentation review and updates in `tickets/<ID>-<slug>/TECHNICAL_WRITING_REPORT.md`.

Do not declare a task fully complete if required tests are missing or have not been run. If a concrete reason blocks their creation or execution (for example, a missing dependency or unavailable browser), record it in the report, list untested scenarios, and state that verification is incomplete; do not present it as successful.

## Target Profile and Quality Criteria

- For a new application and tasks that change operational expectations, record the target profile in `TASK.md`, `ANALYSIS.md`, or `DEPLOYMENT.md`: local prototype/MVP or production candidate/production. If no target profile is specified, explicitly record the assumption and open question; do not present local checks as evidence of production readiness.
- Do not impose one stack, architecture template, set of layers, or infrastructure on every project. Evaluate boundaries based on cohesion, clarity of changes, and the ability to test logic independently; allow smaller projects to use a compact structure with justified exceptions.
- For a production candidate, require evidence of checks relevant to the selected environment and risks: configuration, security, release, recovery, and observability. List unverified areas as limitations. Do not declare an application production-ready based only on a static source review.

## Task Folders and Persistent Context

- Folder format: `tickets/<ID>-<short-slug>/`, for example `tickets/1-task-manager/` or `tickets/APP-123-add-task-status/`. Preserve an external tracker ID; if none exists, assign the next available local numeric ID according to `tickets/README.md`.
- In Git, track only `tickets/README.md` from the `tickets/` directory. Do not stage or commit `tickets/<ID>-<slug>/` or its contents.
- Do not add `tickets/` to `.gitignore` or `.git/info/exclude`: ticket materials must remain visible as untracked files in `git status`. Stage only specific required files by explicit path; do not use `git add -A` or `git add tickets` while local tickets exist in the working tree. Before committing, check staged paths and remove any `tickets/<ID>-<slug>/` paths from the index.
- A slug is a short description of the task in lowercase kebab-case. Use the same ID and slug for the task folder and its related prototype folder.
- `TASK.md` is required and contains the original request, ID, and short title. Store related analysis, test reports, reviews, and other results in the same folder according to `tickets/README.md`.
- First study `TASK.md`, existing analysis, review reports, and related shared documents. Do not overwrite existing task materials.

## Pi Skills and File Responsibilities

| Pi skill / role | File responsibilities |
| --- | --- |
| analyst | DOMAIN.md, BUSINESS_RULES.md, REQUIREMENTS.md, GLOSSARY.md, `tickets/README.md`, `tickets/*/ANALYSIS.md` |
| architector | ARCHITECTURE.md, CONVENTIONS.md, docs/adr/*.md, docs/DESIGN.md |
| frontend-developer | client-side source code, UI, client state and API integration, frontend configuration/build/dependencies |
| backend-developer | server source code, server logic, API.md, and backend configuration/build/dependencies |
| database-developer | DATABASE.md, SQL files, database migrations, docs/DB_PERFORMANCE.md |
| devops | DEPLOYMENT.md, infrastructure configurations, servers, clusters, CI/CD, and deployment |
| security-engineer | SECURITY.md, `tickets/*/SECURITY_REVIEW.md`, security analysis of code and configurations |
| ux-ui-expert | `prototypes/**`, `prototypes/README.md`, docs/UI_GUIDELINES.md, docs/USER_FLOWS.md |
| unit-tester | TESTING.md, docs/UNIT_TESTING.md, creating and maintaining unit tests, auditing and running the full suite, `tickets/*/UNIT_TEST_REPORT.md` |
| ui-tester | UI/E2E section of TESTING.md, UI/E2E tests, docs/UI_TESTING.md, `tickets/*/UI_TEST_REPORT.md` |
| planner | creating and maintaining staged implementation plans in `tickets/*/PLAN.md` |
| technical-writer | README.md, user and developer help/guides in docs/**, `tickets/*/TECHNICAL_WRITING_REPORT.md`; coordinates wording in specialist documents with their owners |
| code-reviewer | CONTRIBUTING.md, docs/CODE_REVIEW.md, `tickets/*/CODE_REVIEW.md` |

Developers are responsible for production code and handing off testable scenarios; they do not create new unit tests or treat their own checks as a substitute for the `unit-tester` report. If `unit-tester` finds after running tests that an existing test is incompatible with the agreed change, the developer may minimally adapt that test based on the tester's feedback; `unit-tester` checks that the test still covers the required behavior and reruns the full suite. `unit-tester` writes new checks for new or changed logic. If verification finds a production-code defect, the tester reports exact steps and actual and expected behavior; the fix is handed to the appropriate developer.

## Project Structure and Shared Artifacts

| Path | Purpose |
| --- | --- |
| `AGENTS.md` | Orchestration rules, required triggers, and responsibility map. |
| `.pi/skills/<role>/SKILL.md` | Specialist role instructions passed by the runner to a separate Pi worker through `--skill`. The presence of a skill does not mean that the role has been assigned. |
| `.pi/skills/subtask-orchestrator/` | Orchestrator skill: plan format, runner that creates a separate Pi process for each step, and result handoff rules. |
| `tickets/README.md` | ID format, task-folder naming, and required/conditional artifacts. |
| `tickets/<ID>-<slug>/TASK.md` | Original request and context for a specific task. |
| `tickets/<ID>-<slug>/PLAN.md` | Staged plan for a feature task or new project, including statuses and actual stage results; the `planner` updates it based on confirmed implementer results. |
| `API.md`, `ARCHITECTURE.md`, `DATABASE.md` | API contract, architecture, and data schema, when those areas exist in the project. Create architecture and data documentation when the project needs it; do not assume optional files already exist. |
| `CONTRIBUTING.md`, `CONVENTIONS.md` | Development rules and technical conventions. |
| `DEPLOYMENT.md`, `SECURITY.md` | Operational, deployment, and security requirements. |
| `TESTING.md` | Required verification levels, run instructions, and reporting. |
| `prototypes/<ID>-<slug>/` | UX/UI prototype and scenario materials for a task with a new or substantially changed user interface. |
| Source-code, test, and migration directories | Determined by the application's actual structure and configuration; study it and create only the minimum necessary structure if the project is empty. |

## Context Handoff and Reports

- Before starting, read `TASK.md`, task-related materials, and applicable specialist documents, including `CONTRIBUTING.md`, `CONVENTIONS.md`, `DEPLOYMENT.md`, `SECURITY.md`, `TESTING.md`, `API.md`, `ARCHITECTURE.md`, and `DATABASE.md`, if they exist and apply to the project. Do not create architecture or database documents just to have the files; add them when they contain lasting decisions or requirements that need to be maintained.
- If a `planner` is assigned for a feature task or new project, study its `PLAN.md` before implementation; after stages finish, pass actual results for plan updates and confirm that statuses are supported by evidence. A routine bug fix does not need a plan unless the orchestrator assigned a `planner` because of its complexity or dependent stages.
- Each child Pi worker returns changed paths, decisions, exact commands and verification results, open questions, and blockers to the orchestrator in the agreed JSON format. A recommendation to involve another role includes its name, task, expected result, and dependencies; the orchestrator decides whether to assign it.
- The orchestrator records persistent decisions and reports in the designated task files and updates relevant shared documents. Do not leave important context only in the conversation or duplicate a single source of truth.
- Do not commit or push changes without the user's direct request.