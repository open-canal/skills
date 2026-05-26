---
tags:
  - project/test
  - doc/test
---

# Test Plan: [Requirement Title]

## Source

- **Primary:** Develop — [[./develop]]
- PRD: [[./prd]]
- Design: [[./design]] (or _none_)
- Version: [[version/x.y.z]] (or _none — requirement-level plan_)

> This test plan is derived from `develop.md` as the primary source. PRD, design, and version links are obtained from develop.md links. Cross-end contract tests require the Contracts section in `develop.md`. If contracts are missing, report as blocked.

## Client Tests

> Remove this section if N/A — no client tasks in develop.md.

### Test Cases

Format: **Given** precondition → **When** action → **Then** expected result.

#### UI States

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| C-01 | Normal render | [precondition] | [action] | [expected] |
| C-02 | Loading | [precondition] | [action] | [expected] |
| C-03 | Empty | [precondition] | [action] | [expected] |
| C-04 | Error | [precondition] | [action] | [expected] |
| C-05 | Permission denied | [precondition] | [action] | [expected] |

#### Navigation

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| C-N1 | [nav scenario] | [precondition] | [action] | [expected] |

#### Offline

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| C-O1 | [offline scenario] | [precondition] | [action] | [expected] |

#### Accessibility

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| C-A1 | [a11y scenario] | [precondition] | [action] | [expected] |

### Acceptance Scenarios (mapped to PRD flow)

| PRD Flow Step | Test ID(s) | Passes When |
|---------------|------------|-------------|
| Main Flow Step 1 | C-01 | [condition] |
| Main Flow Step 2 | C-02 | [condition] |

## Service Tests

> Remove this section if N/A — no service tasks in develop.md.

### Test Cases

#### Contract Tests (mapped to develop.md Contracts)

> Every contract row in develop.md must have a test case here.

| Contract Row | Test ID | Given | When | Then |
|--------------|---------|-------|------|------|
| TBD — derive from develop.md Contracts | TBD | TBD | TBD | TBD |

#### Auth & Authorization

> Remove this section if N/A.

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| S-AU1 | No token | [no auth header] | request | 401 |
| S-AU2 | Wrong role | [non-admin token] | admin-only request | 403 |

#### Persistence

> Remove this section if N/A.

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| S-P1 | Create persists | — | POST | record exists in DB |
| S-P2 | Idempotent create | [existing key] | POST with same key | no duplicate |

#### Error Scenarios

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| S-E1 | Invalid input | [malformed body] | POST | 400 + error body |
| S-E2 | Not found | [missing id] | GET | 404 |

#### Realtime Events

> Remove this section if N/A.

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|
| S-R1 | Event emitted | — | POST success | event pushed to connected clients |

## Cross-End Contract Tests

> Remove this section if N/A — client-only or service-only develop plan. Otherwise mark blocked if contracts are missing. Every contract in develop.md must have at least one cross-end test.

| ID | Contract | Given | When | Then |
|----|----------|-------|------|------|
| TBD — derive from develop.md Contracts | TBD | TBD | TBD | TBD |

## Fixtures & Seed Data

- [Required data before test run]
- [Cleanup steps]

## Execution Notes for Downstream Agents

- Run order: [e.g. S-C01 → S-C02 before C-01]
- Environment: [e.g. local, staging]
- Tools: [e.g. Playwright for client, Supertest for service]

## Evidence Locations

- `assets/test/`
- [Version-level notes path if applicable]

## Blocked Cases

> Report missing implementation details as blockers.

- [ ] [Blocked test group] — reason: [missing spec / contract / endpoint]
