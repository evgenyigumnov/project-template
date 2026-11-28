# User Interface Prototypes

This directory contains reviewable UX/UI artifacts related to project tasks.

## Structure

For a task that adds a visible screen or component, introduces a new user scenario, or substantially changes an existing screen, component, or UI flow, create a directory:

```text
prototypes/<ticket-id>-<slug>/
```

The name must match the task directory in `tickets/`. For example, for `tickets/1-task-manager/`, store the prototype in `prototypes/1-task-manager/`.

Keep at least one reviewable artifact in each directory, such as an HTML prototype, wireframe, or flow diagram. Add a local `README.md` with a brief link to the task and an explanation of how to read the artifact. Do not leave the result only in the conversation.

## Contents and Handoff

The prototype should show the affected screens, key user actions, and states that affect behavior: loading, success, empty results, validation, error, and cancellation, where applicable. State any important accessibility requirements. Do not invent business rules; label unconfirmed decisions as assumptions or open questions.

The UX/UI specialist gives the orchestrator the exact prototype paths, a brief description of the flow and states, and any open questions. The orchestrator passes this context to the frontend developer and UI tester. A prototype is not required for changes with no visible user effect.

Reusable, shared user flows are documented in `docs/USER_FLOWS.md`; details for a specific task remain in that task's prototype directory.
