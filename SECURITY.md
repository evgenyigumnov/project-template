# Security

`security-engineer` owns these requirements and this document. Developers and devops must follow these rules in code, configuration, and infrastructure.

## Authentication and Access

- Verify access rights on the server side for every protected operation.
- Apply the principle of least privilege, and do not treat a hidden UI as protection for a resource.
- Do not weaken authentication or authorization to work around an error or simplify testing.
- For data operations, verify that the subject has access to the specific requested object and context (for example, an organization or project), rather than merely being authenticated.
- For new or substantially changed authentication, authorization, trust boundaries, or public service surfaces, involve `security-engineer` before completing the task. Record threats, decisions, and residual risks in the task or the relevant specialist document.

## Credentials and Sessions

- Do not ship shared, predictable, or built-in login credentials for environments available to users. Demo users and initial passwords are permitted only for local development or isolated tests; they must be explicitly enabled and must not be enabled by default in production configurations. Starting in production with such settings must fail safely.
- Use a credential-storage algorithm supported by the project for passwords; do not develop custom hashing schemes or store passwords in a reversible form.
- Before implementing sessions or tokens, define and document their expiration, scope, revocation, and storage location. For client applications, assess the consequences of XSS and CSRF. Do not store long-lived secrets in storage accessible to scripts without justification and compensating controls.
- If the mechanism uses cookies, configure the security attributes available for them and check request origins; if it uses another mechanism, describe equivalent controls for the selected threat model. Do not treat any single storage method as universally safe for all applications.

## Secrets and Sensitive Data

- Do not add tokens, passwords, private keys, or other secrets to source code, test data, logs, or repository history.
- Use the secret-storage mechanism approved by the project; document the specific mechanism after it has been selected.
- Do not return or log sensitive data without a justified need.
- Separate development, test, and deployed-environment configuration. Do not allow a silent fallback to weak values, test keys, or permissive modes when required configuration is missing.

## Secure Data Handling

- Define trust boundaries and validate untrusted input at every boundary where it becomes trusted. Check data format and meaning, use allowlists where appropriate, and repeat critical checks on the side responsible for protecting the resource.
- Set reasonable upper limits for input size and quantity, processing time, uploads, and query results. Choose values based on product requirements and document them where the contract is described; do not rely only on client-side limits.
- Use safe methods for working with databases, files, and external requests. Do not expose internal errors to the client or include sensitive values in diagnostic messages.
- Protect authentication and resource-intensive operations from brute-force attempts and excessive use with appropriate rate limits, quotas, or other controls. Consider identity, source, and the consequences of blocking legitimate users.
- If a limit stores state in process memory, explicitly state that it applies only to that process. For multiple processes or instances, use a shared mechanism or another solution that enforces the stated limit; do not present a local limit as protection for the entire service.

## Security Checks

- For affected scenarios, check successful access, denied access, bypass through another user's object or context, invalid and excessive input, and abuse where applicable. Regression checks must confirm that protection cannot be bypassed through an adjacent path.
- Use the source-code, configuration, and third-party dependency checks available for the selected stack. Record the tools used and their results; if no suitable automated check exists or it was not run, state the reason and the unchecked risk. Do not require one specific scanner for every technology.
- Before a production candidate, verify that the environment configuration does not enable demo credentials or test keys, secrets are passed using the accepted secure method, transport and session management match the threat model, and abuse controls work at the anticipated deployment scale.

## Prohibited Operations

- Do not disable security checks, TLS verification, or access control for convenience.
- Do not perform destructive data operations or apply changes to live infrastructure without a direct instruction and an agreed plan.
- Do not test for vulnerabilities on systems for which there is no explicit authorization to test.

## Vulnerability Reporting

Send confirmed findings to the main agent and the owner of the affected area. In `tickets/<number>/SECURITY_REVIEW.md`, record severity, path and line, exploitation conditions, impact, and the recommended fix; do not include actual secrets.