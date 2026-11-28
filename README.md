# Pi.dev Project Template

This is a starter template for working with [Pi](https://pi.dev): specialist project skills under `.pi/skills/`, rules for orchestrating child Pi processes, documents on quality, testing, security, and deployment, as well as templates for tasks and UX/UI prototypes. Copy the template into a repository and adapt the materials to its goals, stack, and team.

## Getting Started with the Template

1. Copy the template files, the `.pi/skills/`, `prototypes/`, and `tickets/` directories into the project root.
2. Install Pi CLI and configure a provider by following the [official quickstart](https://docs.pi.dev/latest).
3. Open the project root and launch `pi`, then describe the task. On first launch, Pi may ask you to trust project resources: allow them to load if you trust the template and its `.pi/skills/`.
4. When a task requires assigned roles or multiple stages, the orchestrator creates a JSON plan and launches `.pi/skills/subtask-orchestrator/scripts/run_workflow.py`. Each step runs in a separate child Pi process with the assigned `--skill`; tasks without delegation run in the current Pi session.

## Roles and Pi Skills

- [`analyst`](.pi/skills/analyst/SKILL.md) — analyzes requirements, business logic, the domain, and tasks.
- [`architector`](.pi/skills/architector/SKILL.md) — designs system architecture and defines technical decisions and development standards.
- [`planner`](.pi/skills/planner/SKILL.md) — studies the repository and maintains a plan for new projects and feature tasks.
- [`frontend-developer`](.pi/skills/frontend-developer/SKILL.md) — develops the client side of the application, interfaces, and API integration.
- [`backend-developer`](.pi/skills/backend-developer/SKILL.md) — develops server logic, APIs, and integrations.
- [`database-developer`](.pi/skills/database-developer/SKILL.md) — designs database structures, writes migrations, and optimizes SQL queries.
- [`devops`](.pi/skills/devops/SKILL.md) — configures servers, infrastructure, CI/CD, and application deployment.
- [`security-engineer`](.pi/skills/security-engineer/SKILL.md) — reviews code, architecture, and infrastructure security, and identifies vulnerabilities.
- [`ux-ui-expert`](.pi/skills/ux-ui-expert/SKILL.md) — designs user interfaces, prototypes, and interaction scenarios.
- [`unit-tester`](.pi/skills/unit-tester/SKILL.md) — creates, maintains, and runs unit tests.
- [`ui-tester`](.pi/skills/ui-tester/SKILL.md) — creates and runs UI/E2E tests and checks user scenarios.
- [`technical-writer`](.pi/skills/technical-writer/SKILL.md) — writes and maintains documentation, help, guides, and instructions.
- [`code-reviewer`](.pi/skills/code-reviewer/SKILL.md) — reviews code quality, adherence to standards, and correctness of changes.
- [`subtask-orchestrator`](.pi/skills/subtask-orchestrator/SKILL.md) — plans multi-stage tasks and launches child Pi processes with assigned role skills.

## Documents Included in This Template

Documents already included in the template:

- [`AGENTS.md`](AGENTS.md) — orchestration rules, role assignment, and required checks.
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — development, verification, and review process.
- [`CONVENTIONS.md`](CONVENTIONS.md) — general coding conventions and architectural boundaries.
- [`DEPLOYMENT.md`](DEPLOYMENT.md) — target profiles, environments, and deployment requirements.
- [`SECURITY.md`](SECURITY.md) — secure development requirements and vulnerability-handling rules.
- [`TESTING.md`](TESTING.md) — testing levels, run instructions, and reporting requirements.
- [`API.md`](API.md) — API contract, if the application provides one.
- [`tickets/README.md`](tickets/README.md) — task folder format, required materials, and conditions for reports; maintained by `analyst`.
- [`prototypes/README.md`](prototypes/README.md) — structure and rules for preparing UX/UI prototypes.
- [`.pi/skills/subtask-orchestrator/references/plan-format.md`](.pi/skills/subtask-orchestrator/references/plan-format.md) — JSON plan, role skill assignment, worker JSON report, and runner management.

Documents that roles create or update as needed:

- **`DOMAIN.md`, `BUSINESS_RULES.md`, `REQUIREMENTS.md`, `GLOSSARY.md`** — domain, business rules, requirements, and terminology; maintained by `analyst`.
- **`ARCHITECTURE.md`, `docs/adr/*.md`, `docs/DESIGN.md`** — system architecture, decisions, and design description; maintained by `architector`.
- **`DATABASE.md`, `docs/DB_PERFORMANCE.md`** — data schema, migrations, and database performance; maintained by `database-developer`.
- **`docs/UI_GUIDELINES.md`, `docs/USER_FLOWS.md`** — general interface guidelines and reusable user flows; maintained by `ux-ui-expert`.
- **`docs/UNIT_TESTING.md`, `docs/UI_TESTING.md`** — detailed conventions for unit and UI/E2E testing; maintained by `unit-tester` and `ui-tester`.
- **`README.md`, `docs/GETTING_STARTED.md`, `docs/USER_GUIDE.md`, `docs/HELP.md`, `docs/TROUBLESHOOTING.md`** — introductory instructions, help, guides, and troubleshooting; maintained by `technical-writer`.
- **`docs/CODE_REVIEW.md`** — review criteria for changes; maintained by `code-reviewer`.
- **`prototypes/<ID>-<slug>/README.md`** — description of screens, actions, states, and assumptions for a UX/UI prototype, if it is convenient to document it in Markdown; prepared by `ux-ui-expert`.
- **`tickets/<ID>-<slug>/TASK.md`** — original request and context for each task; created by the orchestrator.
- **`tickets/<ID>-<slug>/PLAN.md`** — stages, dependencies, and actual progress on the task plan; prepared and updated by `planner` based on confirmed results from implementers.
- **`tickets/<ID>-<slug>/ANALYSIS.md`** — requirements and domain analysis; prepared by `analyst`.
- **`tickets/<ID>-<slug>/UNIT_TEST_REPORT.md`** — scenarios, tests, commands, and unit check results; prepared by `unit-tester`.
- **`tickets/<ID>-<slug>/UI_TEST_REPORT.md`** — scenarios, tests, commands, and UI/E2E check results; prepared by `ui-tester`.
- **`tickets/<ID>-<slug>/TECHNICAL_WRITING_REPORT.md`** — sources reviewed, documents updated, or the reason no changes were needed; prepared by `technical-writer`.
- **`tickets/<ID>-<slug>/CODE_REVIEW.md`** — written review of changes, when assigned; prepared by `code-reviewer`.
- **`tickets/<ID>-<slug>/SECURITY_REVIEW.md`** — security review results; prepared by `security-engineer`.