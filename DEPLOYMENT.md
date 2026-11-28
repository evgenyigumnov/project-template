# Build and Deployment

`devops` owns this document and the operational instructions. This template does not choose a language, runtime, platform, cloud provider, or delivery method. Record only decisions supported by the project requirements and its configuration.

## Usage Profile

For the project and any significant change to its target environment, state the expected profile and its boundaries in `TASK.md` or in this document:

- **Local prototype or MVP** — run the application for development, demonstration, or limited use. Do not require a production cluster, multiple regions, 24/7 on-call coverage, or other unconfirmed requirements for this profile. List limitations separately and do not describe them as verified production guarantees.
- **Production candidate** — the application is intended for use in a real target environment. Before launch, define the environment, owners, operational prerequisites, and a verifiable readiness plan.
- **Production** — the application is deployed and tested in the stated environment, with evidence that it works and the necessary operational procedures in place. A static source-code review alone does not confirm production readiness.

If the profile is undefined, record that as an open question or assumption in the task. Do not silently raise the profile or infer production readiness solely from the presence of configuration files or a successful build.

## Project and Environment State

Describe the target environments, their purposes, limitations, and confirmed prerequisites. Distinguish development, test, and deployed-environment configuration. Do not perform real deployment operations based on assumptions.

## Build and Artifacts

Add confirmed build commands, required tool versions, generated artifacts, and configuration requirements. Use the actual project files and configured checks as the source for commands. Do not copy commands from another stack and present them as universal.

## Configuration and Secrets

Describe the required environment variables or other settings, their defaults, and how to pass secrets securely. Do not write secret values in the documentation. For a production candidate, verify that required settings are explicitly configured and that demo credentials, test keys, and permissive modes are not enabled.

Specify secure transport and session settings where applicable. For public-facing or multi-user services, describe protection against excessive use. If that protection stores state only in process memory, record its scope; when multiple instances run, state how the shared limit is enforced.

## Production Candidate Criteria

Before launch in a real environment, check the applicable items and attach actual evidence, or explicitly list blockers and unverified risks:

- the target environment, operations owner, configuration method, and secure handling of secrets are defined;
- the build and applicable automated checks are reproducible; configured security and dependency checks have run, or the reason they are absent is recorded;
- transport, authentication, authorization, input handling, and abuse limits match the threat model and expected scale;
- if the application stores important data, compatibility and schema-change procedures, backups, and tested restoration are defined;
- health checks, collection of diagnostic data without leaking sensitive information, and monitoring for critical failures are defined;
- release and rollback procedures are defined. For changes affecting data or external interfaces, describe how to restore a compatible state.

This is a readiness check for the stated scenario, not a universal technology checklist. Mark items that do not apply to the project as not applicable and briefly explain why; do not mark items as passed without evidence.

## CI/CD and Release

Document only existing or approved pipelines, build and delivery stages, required checks, artifacts, and owners. For a production candidate, state which CI results and target-environment checks are required before release. Do not claim a pipeline is configured until it exists and its execution has been confirmed.

## Deployment, Rollback, and Operations

After choosing the target environment, record the deployment and rollback sequence, health checks, required data operations, monitoring, backup, and restoration procedures, limited to what applies to the project. Include commands and environment limitations only after verifying them against the actual configuration. Do not operate live infrastructure without a direct assignment and an agreed plan.