# Pi worker workflow plan format (version 1)

The runner executes a plan sequentially. Every step is a new one-shot Pi process in the same workspace, started with the skill named by that step.

## Plan schema

Pass a UTF-8 JSON object with these fields:

| Field | Type | Requirement |
| --- | --- | --- |
| `version` | integer | Must be exactly `1`; `true` is not accepted as an integer. |
| `goal` | string | Required and nonempty after trimming. |
| `context` | string or `null` | Optional shared context; omitted or `null` defaults to empty. |
| `steps` | array | Required, with 1–10 step objects in execution order. |

Every step has these fields:

| Field | Type | Requirement |
| --- | --- | --- |
| `id` | string | Required, unique, and matches `^[a-z0-9][a-z0-9_-]{0,63}$`. |
| `title` | string | Required and nonempty after trimming. |
| `skill` | string | Required; Agent Skills name of at most 64 lowercase letters, digits, and single hyphens, resolving to a role skill at `.pi/skills/<skill>/SKILL.md` in the workspace. Do not assign `subtask-orchestrator` to a worker. |
| `task` | string | Required, bounded, and nonempty after trimming. |
| `context` | string or `null` | Optional step-specific context; omitted or `null` defaults to empty. |
| `acceptance` | array of strings | Required and nonempty; every item must be nonempty. |
| `depends_on` | array of strings | Required; may be empty. IDs must be unique and refer only to earlier steps. |

Steps run in listed order. Dependencies declare which earlier results are handed to a worker; they do not reorder or parallelize the plan. Unknown object properties are ignored. Example:

```json
{
  "version": 1,
  "goal": "Add and document a small feature",
  "context": "Keep the change compatible with the existing project.",
  "steps": [
    {
      "id": "implement",
      "title": "Implement the feature",
      "skill": "backend-developer",
      "task": "Add the requested feature and inspect related code.",
      "context": "Use the existing project conventions.",
      "acceptance": ["The feature is implemented", "Relevant checks pass"],
      "depends_on": []
    },
    {
      "id": "document",
      "title": "Document the feature",
      "skill": "technical-writer",
      "task": "Document the interface and how to use it.",
      "acceptance": ["Usage is documented"],
      "depends_on": ["implement"]
    }
  ]
}
```

## Worker result contract

Every Pi worker must finish with raw JSON as its final assistant text. Do not wrap it in a Markdown code fence or add commentary before or after it. The object must contain exactly these fields:

```json
{
  "status": "completed",
  "summary": "Implemented the assigned step.",
  "files_changed": ["src/example.py"],
  "verification": [
    {"command": "python -m compileall src", "outcome": "passed"}
  ],
  "risks": []
}
```

`status` must be `completed`, `blocked`, or `failed`; `summary` must be nonempty. `files_changed` and `risks` are arrays of strings. `verification` is an array of objects containing only string fields `command` and `outcome`. The runner rejects malformed JSON, duplicate keys, Markdown wrappers, extra or missing fields, and invalid field types. Only `completed` counts as success.

## Skill assignment and context

The runner resolves each skill only inside the workspace at `.pi/skills/<skill>/SKILL.md`; missing or invalid skills fail before Pi starts. It launches Pi with `--mode json --no-session --no-skills --skill <resolved-skill-file>`, plus `--model MODEL` when one is configured. Pi's CLI allows an explicit `--skill` path alongside `--no-skills`, so the child receives the selected role skill while automatic discovery of other skills is disabled. The first line of the prompt tells the child to read and apply that skill before any other work.

Pi JSON mode writes a JSONL event stream to stdout and exits after one invocation. The runner extracts the final assistant text and accepts it only if it parses as the exact result object above; a nonzero process exit also fails the attempt. Workers retain `AGENTS.md` context discovery because the runner does not pass `--no-context-files`.

Each worker receives the goal, shared context, the ordered workflow map, its task and acceptance criteria, and detailed results only for its declared dependencies. Dependency details are truncated to `--max-summary-chars` across all dependencies. Later steps can inspect earlier changes because all workers share the same workspace.

## Failure handling and controls

The runner attempts each step once plus the configured retry count; retry count is limited to `0` or `1`. Timeouts and stalled commands are not retried. By default, after a step remains unsuccessful, later steps are reported as not run. With `--continue-on-error`, remaining steps run in plan order, including declared dependencies whose worker failed; the final workflow still reports `failed`. A failed attempt may leave partial changes; the runner does not roll them back.

| Option | Default | Behavior |
| --- | --- | --- |
| `--workspace DIR` | `.` | Existing shared worker workspace, resolved before execution. |
| `--dry-run` | off | Validates the plan and role skills, then prints commands and controls without launching Pi. |
| `--timeout-seconds N` | `1800` | Hard maximum runtime for an individual worker. |
| `--timeout-checkpoint-seconds N` | `600` | Inspect progress and active shell commands at each interval before the hard timeout. |
| `--retries N` | `1` | Retries after failure; only `0` or `1` is allowed. |
| `--continue-on-error` | off | Runs later steps after a failure; the workflow still reports failure. |
| `--max-summary-chars N` | `16000` | Positive character cap for dependency results in each prompt. |
| `--artifacts-dir DIR` | not retained | Optional new directory outside the workspace for prompts, diagnostics, results, and report; an existing destination is refused. |
| `--live-log PATH` | system temp directory | Write workflow activity to one live log; the runner prints its path and a platform-appropriate follow command at startup. |
| `--pi-bin PATH` | `pi` | Pi executable name or executable path. |
| `--model MODEL` | `PI_WORKFLOW_MODEL`, then Pi's configured default | Model passed as `--model MODEL`; the CLI option takes precedence over the environment variable. |

The runner invokes Pi with an argument array and supplies the prompt over stdin; it does not start a shell. Child environment adds `PI_WORKFLOW_CHILD=1` and `PI_WORKFLOW_DEPTH=1`. The runner refuses to start if either variable shows it is already running as a child. It preserves other environment variables.

The live log contains readable summaries of orchestrator and worker activity, including commands and short output or assistant-response summaries. It is separate from optional retained artifacts and may contain sensitive project details. Pi CLI can run commands and modify files with the current user's permissions; neither Pi nor this runner is a sandbox.

## Examples

Run from the project root after writing the plan to a temporary JSON file outside the workspace.

Windows PowerShell:

```powershell
py -3 .\.pi\skills\subtask-orchestrator\scripts\run_workflow.py <plan.json> --workspace . --dry-run
py -3 .\.pi\skills\subtask-orchestrator\scripts\run_workflow.py <plan.json> --workspace .
```

macOS or Linux:

```bash
python3 .pi/skills/subtask-orchestrator/scripts/run_workflow.py <plan.json> --workspace . --dry-run
python3 .pi/skills/subtask-orchestrator/scripts/run_workflow.py <plan.json> --workspace .
```
