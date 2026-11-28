# API

The API contract is documented here as endpoints are introduced. In the current template, specific routes and the transport have not yet been defined; do not treat examples or future sections as implemented behavior.

The owner of this document and the API contract is `backend-developer`. Changes that affect client integration are coordinated with `frontend-developer`; changes to authentication and authorization are coordinated with `security-engineer`.

## Overview

- API purpose: to be defined together with the application's requirements.
- Base URL, transport, and version: not yet defined.

## General Rules

For each endpoint, document the method and path, purpose, authorization requirements, request parameters and body, successful responses, errors, and compatibility constraints. Specify values only after they have been confirmed by the implementation or requirements.

## Authentication and Authorization

Not yet defined. After selecting a mechanism, describe the identity model, access requirements, and behavior when access is denied here, consulting `SECURITY.md`.

## Request and Response Formats

Not yet defined. Record the accepted formats, validation rules, pagination, and date representation if they are introduced in the project.

## Errors

Not yet defined. For each error type, specify the HTTP status (if applicable), a stable code, and a client-safe description. Do not include secrets or internal data in responses.

## Endpoints

| Method | Path | Access | Purpose |
| --- | --- | --- | --- |
| — | No endpoints documented yet | — | — |

## Compatibility and Changes

Before releasing the API, define versioning and client support rules. Coordinate incompatible changes with consumer owners and document them here before implementation.
