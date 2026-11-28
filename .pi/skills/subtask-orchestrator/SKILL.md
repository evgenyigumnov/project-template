---
name: subtask-orchestrator
description: "Plans and runs repository work by launching sequential, one-shot Pi CLI workers with explicit project role skills. Use when AGENTS.md requires role delegation or a task has multiple bounded steps, role handoffs, or dependencies; handle tasks that need no delegation in the current Pi session."
---

# Subtask orchestrator for Pi

Use the current Pi session as the planner and reviewer. The runner at `scripts/run_workflow.py` starts a separate Pi process for each planned step and explicitly supplies that step's role skill.

## Workflow

1. Use this workflow when `AGENTS.md` requires specialist role delegation or when separate worker contexts and ordered handoffs materially help. Handle questions and small changes that need no role delegation directly in the current Pi session.
2. Read the repository's `AGENTS.md`, task materials, and relevant project docs. For implementation work, create the ticket folder and `TASK.md` first as required by `AGENTS.md` and `tickets/README.md`.
3. Prepare a version 1 JSON plan with 1–10 steps. Every step has a unique ID, title, `skill`, bounded task, nonempty acceptance criteria, and dependencies only on earlier steps. The `skill` must match a directory under `.pi/skills/` that contains `SKILL.md`. Put confirmed repository findings, requirements, constraints, paths, decisions, and relevant edge cases in the plan; workers do not inherit this conversation.
4. Read `references/plan-format.md`. Save the plan in a temporary JSON file outside the repository and run the runner with `--dry-run`. The dry run validates the plan, confirms each role skill exists, and shows each child command without launching Pi.
5. Before launching workers, tell the user the workflow goal and ordered step titles with a short task description and dependencies. Continue directly unless the user asks to review or revise the plan.
6. Run the script from the target repository root.

   Windows PowerShell:

   ```powershell
   py -3 .\.pi\skills\subtask-orchestrator\scripts\run_workflow.py <plan.json> --workspace .
   ```

   macOS or Linux:

   ```bash
   python3 .pi/skills/subtask-orchestrator/scripts/run_workflow.py <plan.json> --workspace .
   ```

   Add `--model <model-id>` only when a known model should override Pi's configured default. Otherwise, the runner uses `PI_WORKFLOW_MODEL` when set, then Pi's configured model. Use `--timeout-seconds`, `--timeout-checkpoint-seconds`, `--retries 0|1`, `--max-summary-chars`, `--pi-bin`, or `--artifacts-dir` only when the task calls for them. Artifacts are optional; the runner refuses to overwrite an existing destination and requires it to be outside the workspace.
7. The runner executes steps sequentially in the listed order. `depends_on` controls which completed results are included in a worker's prompt; it does not reorder steps or run them in parallel. A worker receives the workflow map, shared context, its task and acceptance criteria, and details only for declared dependencies.
8. Each child starts in one-shot JSON mode, without a saved session or automatic skill discovery, and receives exactly its assigned role skill through Pi's `--skill` option. The first instruction in its prompt requires it to read and apply that skill before other work. Normal `AGENTS.md` context discovery stays enabled. The child must return raw JSON matching the result schema in `references/plan-format.md`.
9. The runner streams readable summaries from the orchestrator and every worker attempt into one flushed live log. It prints the log path and a follow command when execution starts. The default log is in the system temporary directory; use `--live-log <path>` to choose another location. The log can contain commands and project details, so treat it as potentially sensitive. Once started, share the printed path and follow command with the user, then wait for the runner to finish. Do not repeatedly poll, reread the log, or send timed progress messages unless the user asks or troubleshooting requires it.
10. A non-timeout failure can use at most the configured retry. Timed-out or stalled commands are not retried. By default, later steps do not run after a failed step; `--continue-on-error` runs remaining steps but the workflow still reports failure. Do not improvise additional worker runs after the runner exits. Review its report and the actual workspace diff, verify requested changes proportionately, and record unresolved checks or blockers.

## Prerequisites and boundaries

- The Pi CLI and a provider configured for Pi must already be available. This skill does not install Pi or configure credentials.
- Project-local `.pi` resources are governed by Pi project trust. Start from a Pi session where the user has trusted the repository; non-interactive runs without a saved trust decision may skip project skills. The runner does not silently add `--approve`.
- Pi tools run with the permissions of the user. The runner is not a security sandbox; use only in an appropriately trusted workspace.
- Workers share the target workspace and run one at a time. A failed attempt may leave partial changes; the runner does not roll them back. Inspect the workspace before retrying or continuing.
- Child Pi processes receive `PI_WORKFLOW_CHILD=1` and `PI_WORKFLOW_DEPTH=1`. The runner refuses nested workflow starts.
- Do not pass secrets in plans or task files. Optional retained artifacts and the live log may contain prompts, source context, command output, and project details.

## References

- [Version 1 plan, worker result, and CLI controls](references/plan-format.md)
- [Pi skills: project discovery and explicit skill loading](https://docs.pi.dev/latest/skills)
- [Pi CLI: JSON mode, sessions, `--skill`, and `--no-skills`](https://docs.pi.dev/latest/cli)
- [Pi JSON event stream](https://docs.pi.dev/latest/json)
- [Sequential Pi workflow runner](scripts/run_workflow.py)
