# Plan — 1: Pi.dev Workflow Migration

Status: Complete. Updated: 2026-11-28.

## Repository Overview

Confirmed: the repository is a documentation and workflow template for Harness AI. Specialist profiles were stored in the former role-profile files; orchestration rules are in `AGENTS.md`; the template overview is in `README.md`. This repository contains no application or product source code. The task changes the orchestration template and profile format.

## Stages

### 1. Move Roles into Pi Skills

- Owner: orchestrator.
- Areas: former role-profile files, `.pi/skills/<role>/SKILL.md`.
- Dependencies: none.
- Criterion: every existing role is available as a separate Pi skill with its specialization and responsibility boundaries preserved.
- Status: complete.
- Actual result: instructions for 13 roles were moved into `.pi/skills/<role>/SKILL.md`; the former role-profile files were removed. The role names were preserved: analyst, architector, backend-developer, code-reviewer, database-developer, devops, frontend-developer, planner, security-engineer, technical-writer, ui-tester, unit-tester, ux-ui-expert.

### 2. Add the Orchestrator Skill and Pi Runner

- Owner: orchestrator.
- Areas: `.pi/skills/subtask-orchestrator/`.
- Dependencies: stage 1.
- Criterion: the JSON plan assigns a role to each step; the runner passes it to the child Pi through `--skill`, requires the role skill to be loaded as the first instruction, passes dependency results, and returns a report.
- Status: complete.
- Actual result: `.pi/skills/subtask-orchestrator/SKILL.md`, `references/plan-format.md`, and `scripts/run_workflow.py` were added. The plan requires a `skill` for every step; the runner resolves it only inside the workspace, passes it to the child Pi through `--skill`, starts the prompt by requiring the role skill to be read, and collects a JSON report. Processes are one-shot and sequential; dependency handoffs, retry and timeout controls, and a recursion guard are included.

### 3. Rewrite the Template Instructions for Pi

- Owner: orchestrator.
- Areas: `AGENTS.md`, `README.md`, `tickets/README.md`, and other references to the former role profiles.
- Dependencies: stages 1–2.
- Criterion: the documentation refers to Pi skills and the runner, reflects the actual sequential execution, and no longer requires legacy subagent execution.
- Status: complete.
- Actual result: `AGENTS.md`, `README.md`, and `tickets/README.md` were updated for Pi skills and the child Pi workflow. A search of the main tracked documents no longer finds references to former role-profile artifacts, subagent execution, or Harness AI.

### 4. Verify the Final Changes

- Owner: orchestrator.
- Areas: the diff on branch `pi`.
- Dependencies: stages 1–3.
- Criterion: the changed-file list and consistency of links and instructions were checked; no tests were added or run.
- Status: complete.
- Actual result: 13 role skills and the orchestrator skill with `SKILL.md` were found; calls to changed runner functions, `--skill` commands, and report handoff were checked. `git diff --check` found no whitespace errors (Git emitted only a warning about LF/CRLF conversion). Automated tests were not run.