# Development Conventions

This document records general conventions for code style, naming, and technical patterns. The owner is `architector`; frontend-developer and backend-developer report recurring conventions in their areas to maintain a single source of truth.

## Source priority

1. Follow the project's current formatters, linters, and configuration.
2. Preserve the style and structure of the nearest existing code.
3. If conventions have not yet been established, propose a minimal convention and record it here after agreement.

## Naming and structure

- Use names that clearly reflect the purpose of an entity.
- Group code around cohesive functionality and give each module a clear responsibility. A module can be any organizational unit accepted by the stack: a function, component, package, service, or another suitable element.
- Separate entry points and external boundaries from application rules where this makes the code easier to understand and change independently. For example, a request handler, screen, or command usually transforms input, delegates work to the appropriate logic, and forms a result; complex rules and operations with external systems should not silently accumulate in one entry point.
- Keep each business rule and invariant in one clear place. Do not duplicate it across the interface, request handler, and data store; client-side checks do not replace checks at a trusted server boundary, if one exists.
- Keep module boundaries explicit: callers should use the interface intended for that purpose instead of directly changing another module's internal state. Add layers and abstractions only when they provide a clear benefit—for example, isolating an external resource, simplifying independent changes, or making complex logic easier to understand.
- During review, assess coupling and the cost of changes: if one function affects many unrelated modules, or one module is regularly changed for different reasons, check whether its responsibilities are mixed. This is a signal to investigate, not an automatic requirement to split it.
- Do not create abstractions or duplicate existing logic without a need.
- Line count alone does not determine the quality of boundaries: a compact file can contain several unrelated workflows, while a long file can be one cohesive table or specification. If a file noticeably exceeds about 500 lines, review its cohesion, the number of reasons it changes, the clarity of its interface, and whether its parts can be changed independently. Do not split it mechanically or add separate layers just to reduce its size.

## Checking responsibility boundaries

Before completing a substantial change, check the applicable points below and record justified exceptions in the change description:

- Entry points and external-system adapters remain clear coordinators of a workflow, not containers for all application logic.
- Rules and state transitions belong to the module responsible for the corresponding behavior and do not diverge across multiple layers.
- Interactions with external systems and data stores go through a clear boundary when separation genuinely helps maintainability or makes the dependency easier to replace.
- Public module interfaces are small enough that callers do not depend on incidental implementation details.
- The structure fits the scale of the task and uses the natural tools of the chosen stack, without imposing one architecture pattern on every project.

These are criteria for a substantive review, not a requirement to create a separate layer for every point. For a small script, prototype, or compact service, several responsibilities may remain together if they are closely related, their boundaries are clear, and further separation would add more complexity than value.

## Errors, configuration, and dependencies

- Handle errors at a level that can recover safely or communicate useful context.
- Keep configuration separate from logic; do not put secrets in source code or the repository.
- Add dependencies only when justified, and consider their maintenance and security risks.

## Deviations

If a task requires deviating from an accepted convention, state the reason in the change and, if needed, propose updating this document.