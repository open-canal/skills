# Development Documentation Standard

## Develop

Use `develop` with `create` to generate a technical plan for a selected demand PRD. Use `develop` with `update` to update an existing technical plan after PRD or design changes.

The development artifact belongs in the same requirement pool:

```text
modules/<module-slug>/requirements/<requirement-slug>/develop.md
```

The output should be suitable for downstream client, service, and database AI agents to execute development tasks in their own code repositories.

## Inputs

Read:

- requirement `prd.md`;
- requirement `design.md` when UI or interaction behavior exists;
- `.open-canal/development-standards/` files (platform, client, service, database, testing) — apply all conventions that constrain the plan;
- linked `version/x.y.z.md` when iteration scope matters;
- existing `develop.md` when updating;
- project architecture or repository routing docs when available.

If design is required but missing, report that gap before writing implementation-specific tasks.

## Development Plan Requirements

Use `.open-canal/templates/develop.md` as the document skeleton. `develop.md` must include sections only when relevant:

- source PRD, design, and version links (version when present);
- implementation goal;
- **contracts** (required when client and service interact) — endpoint-level API contracts and realtime event contracts;
- **Client Design** — technical design decisions for client-side behavior (state, navigation, cache, networking);
- **Client Tasks** — single-scope implementation tasks for client work;
- **Service Design** — technical design decisions for service-side behavior (endpoints, auth, persistence);
- **Service Tasks** — single-scope implementation tasks for service work;
- database plan (entities, migrations, database tasks);
- cross-end concerns;
- data ownership and source-of-truth rules;
- offline, sync, realtime, permissions, and error handling;
- migration or compatibility notes;
- downstream agent task list;
- blockers and open questions.

### Contracts Section

Every API interaction between client and service must be listed as a contract row:

| Element | Required | Description |
|---------|----------|-------------|
| Endpoint | Yes | Full path, e.g. `/api/v1/resource` |
| Method | Yes | GET, POST, PUT, PATCH, DELETE |
| Request | Yes | Query params, path params, body schema |
| Response | Yes | Status code, body schema |
| Auth | Yes | Bearer token, session, none |
| Errors | Yes | Status codes and when they occur |

Realtime events must include direction (server→client or client→server), payload schema, and trigger condition.

## Client Section

Include client UI state, navigation, permissions, local cache, networking, offline behavior, and feature-local assets when relevant.

Do not redefine product behavior. Link back to PRD and design for user-facing flow.

## Service Section

Include API endpoints, request/response contracts, auth, authorization, WebSocket or realtime events, error behavior, and state transitions when relevant.

Derive current user identity from trusted auth context. Do not trust user IDs in request bodies for authorization.

## Database Section

Include tables/entities, relations, ownership, indexes, uniqueness, idempotency, retention, and migration impact when relevant.

Shared records should have one backend source of truth. Local cache and external sync systems need explicit boundaries.

## Task Granularity

Each downstream agent task must be:

- **Independently executable** — one task does not require another task's in-progress state to function.
- **Single-scope** — one task touches only one of client, service, or database.
- **Sized for one agent session** — roughly 5-15 minutes of implementation work. A task should map to: one UI component + its state handling, or one API endpoint + persistence, or one migration script.
- **Verifiable** — the task includes a concrete testable outcome (e.g. "endpoint returns 200 with schema X" not "endpoint works").

The task list must be grouped into parallel execution batches. Tasks within the same group share no mutable state and can run in parallel (across one or more agent sessions). Groups execute sequentially — each group may depend on earlier groups completing. The downstream agent handoff table documents group membership, dependencies, and prerequisites.

## Update Rules

When updating development docs after PRD or design changes:

1. Compare changed user-facing behavior before editing technical tasks.
2. Update affected client/service/database sections together.
3. Re-validate contracts against the updated PRD/design.
4. Keep downstream agent tasks small enough to execute independently.
5. Report any stale task, contract, or data model that conflicts with the latest PRD/design.
