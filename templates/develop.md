---
tags:
  - project/develop
  - doc/develop
---

# Develop: [Requirement Title]

## Source

- PRD: [[./prd]]
- Design: [[./design]] (or _none — no UI involved_)
- Version: [[version/x.y.z]] (or _none_)

## Implementation Goal

[One sentence: what technical capability does this deliver?]

## Contracts

> Remove this section if N/A — no client-service interaction. Otherwise: shared agreements between client and service. Every endpoint below maps to at least one downstream task.

### API Contracts

| Endpoint | Method | Request | Response | Auth | Errors |
|----------|--------|---------|----------|------|--------|
| `TBD — derive from PRD user flow` | GET | `{ TBD }` | `{ TBD }` | TBD | TBD |
| `TBD` | POST | `{ TBD }` | `{ TBD }` | TBD | TBD |
| `TBD` | DELETE | — | `TBD` | TBD | TBD |

### Realtime Events

> Remove this subsection if N/A.

| Event | Direction | Payload | Trigger |
|-------|-----------|---------|---------|
| `TBD` | TBD | `{ TBD }` | TBD |

## Client Design

> Remove this section if N/A — no client work in this plan.

> The technical design of client-side behavior. Defines architecture decisions that constrain implementation tasks.

### State & Navigation

- [State / route change description]
- [Permission gates]

### Local Cache

- [What gets cached, invalidation rules]

### Networking

- [Which endpoints are called from where]
- [Offline behavior]

## Client Tasks

> Remove this section if N/A — no client work in this plan.

> Tasks below are single-scope client implementation items, independent from service/database work.

| # | Task | Files | Estimate |
|---|------|-------|----------|
| C1 | [Task description — single UI component, state handling, or endpoint integration] | `src/...` | ~X min |
| C2 | [Task description] | `src/...` | ~X min |

## Service Design

> Remove this section if N/A — no service work in this plan.

> The technical design of service-side behavior. Defines architecture decisions that constrain implementation tasks.

### Endpoints

- [Endpoint-level logic per contract row above]

### Auth & Authorization

- [Token handling, role checks, derived identity rules]

### Persistence

- [What is stored, idempotency keys, transaction boundaries]

## Service Tasks

> Remove this section if N/A — no service work in this plan.

> Tasks below are single-scope service implementation items, independent from client/database work.

| # | Task | Files | Estimate |
|---|------|-------|----------|
| S1 | [Task description — single endpoint or data operation] | `src/...` | ~X min |
| S2 | [Task description] | `src/...` | ~X min |

## Database Plan

> Remove this section if N/A — no database work in this plan.

### Entities

| Table / Collection | Key Fields | Indexes | Ownership |
|--------------------|------------|---------|-----------|
| `<table_name>` | `id (PK)`, `<owner_col>`, `created_at` | `<owner_col>` | service |

### Migration Notes

- [New tables, altered columns, backfill steps]

### Database Tasks

> Tasks below are single-scope database items, independent from client/service work.

| # | Task | Files | Estimate |
|---|------|-------|----------|
| D1 | [Migration / schema task] | `migrations/...` | ~X min |

## Cross-End Concerns

- **Data source of truth:** [which service owns which data]
- **Error propagation:** [how errors flow from service to client]
- **Offline / sync:** [strategy for disconnected operation]

## Blockers & Open Questions

- [ ] [Question / decision needed]
- [ ] [Missing dependency]

## Downstream Agent Handoff

> Tasks above are grouped into parallel execution batches. Client, service, and database tasks in the same group share no mutable state and can run in parallel. Groups execute sequentially — later groups may depend on earlier groups completing.

| Group | Tasks | Prerequisites |
|-------|-------|---------------|
| 1 | C1, S1, D1 | — |
| 2 | C2, S2 | Group 1 complete |
