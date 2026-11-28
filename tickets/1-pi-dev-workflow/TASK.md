# TASK — 1: Pi.dev workflow migration

ID source: local numbering, next available number.

## Original user request

> In `C:\Users\igumn\project-template\`, create a branch named `pi`
> and refactor this repository on that branch
> so that it is tailored for pi.dev.
> Therefore, instead of using subagents, make it launch child Pi processes. Read the skill at `C:\Users\igumn\subtask4llm\skills\subtask4llm-pi\`, use it as a foundation, and create a skill on this topic inside `C:\Users\igumn\project-template\`.
> In short, the existing subagents should become skills and be invoked when subtasks are created. The orchestrator should create a Pi instance whose first instruction is to apply the assigned skill (agent role) and complete the task.

## Acceptance criteria

- Work is carried out on a separate `pi` branch.
- The roles from the existing TOML agent profiles are represented as project Pi skills in `.pi/skills/<role>/SKILL.md`.
- The project has its own skill for orchestrating Pi processes, with a runner script and plan format.
- A role skill can be selected for each step; the runner launches a separate child `pi` process with that skill and instructs it to apply the skill before doing the task.
- The main instructions and README no longer describe subagents as the workflow mechanism.

## Assumptions

- The existing roles and their specializations are preserved; only their format and launch mechanism change.
- Specialized workers are invoked sequentially through the Pi CLI; Pi and a configured provider are already installed for the user.
- The artifacts for this task remain local and are not added to Git, according to `tickets/README.md`.