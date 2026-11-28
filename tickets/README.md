# Task and Ticket Folders

This directory stores context for individual tasks. Before starting analysis or implementation, the orchestrator creates a separate folder for the task and saves the original description in it. Do not leave the task number and requirements only in the conversation.

## Git Tracking

Only this `README.md` is tracked in the repository. Do not add individual ticket directories or files to commits, and do not add them to `.gitignore` or `.git/info/exclude`: new materials must remain visible as untracked files in `git status`. While local tickets exist in the working tree, do not use `git add -A` or `git add tickets`; add only the required files to the index by their explicit paths, and check the staged paths before committing. There is no technical protection against manually adding ticket files to the index: if you add them manually, Git will allow them to be included in a commit.

## ID and Folder Name

Use this format:

```text
tickets/<ID>-<short-slug>/
```

- If the task came from an issue tracker, preserve its ID unchanged. For example, `APP-123` and the task “login and task list” result in `tickets/APP-123-login-and-task-list/`.
- If only a number is specified, use it as the ID: task No. 1, “task manager,” results in `tickets/1-task-manager/`.
- If no ID is specified, the orchestrator assigns the next available sequential local number based on the existing folders. Do not create a parallel folder for the same ID.
- The short description should contain 2–6 specific words in lowercase, separated by hyphens. Use Latin characters; you may translate or transliterate a Russian title. For example, `APP-123-login-and-task-list`.
- If a folder for this ID already exists, continue working in it. Do not rename the folder when the task wording changes: the ID remains the primary identifier.

## Task Files

`TASK.md` is required for every task. Create it before handing work to child Pi processes. Include:

- the ID and a short task title;
- the ID source: external tracker or local numbering;
- the user's original request verbatim;
- verifiable acceptance criteria, if they can be determined. Distinguish confirmed requirements from assumptions and open questions.

Add specialist artifacts when the corresponding work is performed:

| File | When to create | Owner |
| --- | --- | --- |
| `ANALYSIS.md` | Requirements or domain analysis was performed | analyst |
| `PLAN.md` | A planner is assigned to a feature task, a new project, or a separately designated large, cross-module, or multi-stage bug fix; contains the plan and current status of its stages | planner |
| `UNIT_TEST_REPORT.md` | Code behavior changed; records coverage, test files, exact commands, results, and execution limitations | unit-tester |
| `UI_TEST_REPORT.md` | Visible UI behavior or a user scenario changed; records scenarios, test files, exact commands, results, and execution limitations | ui-tester |
| `TECHNICAL_WRITING_REPORT.md` | Code, behavior, API/contracts, operations, user scenarios, or documentation changed; records reviewed sources, updated documents or why no edits were needed, as well as questions and limitations | technical-writer |
| `CODE_REVIEW.md` | A written review was assigned for the task | code-reviewer |
| `SECURITY_REVIEW.md` | A security review was performed | security-engineer |

A UX prototype is not needed if the task does not add or change a visible screen, component, or user scenario. For a new or substantially changed visible screen, component, or UI scenario, the UX/UI specialist prepares a prototype in `prototypes/<ID>-<short-slug>/`; the orchestrator records this path in `TASK.md` or `ANALYSIS.md`.

### Work Plan (`PLAN.md`)

When a planner is assigned according to `AGENTS.md`, they study the task and available codebase, then create `PLAN.md` in the ticket folder before implementation. A routine local bug fix does not by itself require a planner; one may be assigned separately for a large, cross-module, or multi-stage fix. A new project is planned even if no source code exists: the plan records the absence of a codebase and the available source materials.

The plan must contain:

- a brief scope, verified sources, and a conclusion about the state of the codebase; if no source code exists, state this explicitly;
- stages in execution order, their tasks and outcomes, responsible implementers, dependencies, and verifiable completion criteria;
- the current status of each stage: `Not started`, `In progress`, `Completed`, or `Blocked`;
- assumptions, open questions, and blockers if they affect the sequence or scope of work.

`PLAN.md` is an evolving record of progress. The planner updates statuses after an implementer or the orchestrator confirms a stage result and cites verifiable evidence (for example, changed paths, reports, and actual verification results). If findings change the stages or dependencies, the planner records what changed and why; completed steps and previous decisions are not erased without explanation. A blocked or revised plan is not marked completed. Other ticket reports retain their separate responsibilities and are not replaced by `PLAN.md`.

Store tests in the project's established test-directory structure, not in `tickets/`. Maintain shared rules and decisions in the relevant documents; do not copy them into every ticket. Do not put secrets, passwords, or real personal data in a ticket.

The acceptance criteria and statuses in `TASK.md` must let the orchestrator determine which specialist checks are required and what still prevents the task from being completed. A report of an unperformed check must state the specific reason; do not mark it as successful.