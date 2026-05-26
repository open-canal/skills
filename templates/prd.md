---
tags:
  - project/requirement
  - doc/prd
status: draft
---

# [Requirement Title]

## Summary

[One sentence: who needs what and why.]

## Actor

- **Primary user/role:** [e.g. registered user, admin, guest]
- **Permissions needed:** [e.g. authenticated, admin-role]

## Scenario & Problem

[What triggers the need? What pain does this solve? Context in 1-2 paragraphs.]

## Goal

[What the user achieves. One sentence.]

## Non-Goals

- [Explicitly excluded behavior A]
- [Explicitly excluded behavior B]

## User Flow

### Preconditions

- [What must be true before the flow starts.]

### Main Flow

1. [Step 1 — user action or system trigger]
2. [Step 2]
3. [Step N — final step]

### Success State

- [What the user sees or the system state when the main flow completes successfully.]

### Alternate Flows

#### Failure

- [What happens on error: message, recovery path.]

#### Empty State

- [What the user sees when no data exists.]

#### Permission Denied

- [What the user sees when they lack access.]

#### Cancellation

- [What happens when the user cancels mid-flow.]

## Acceptance Criteria

- [ ] [Criterion 1 — testable, binary]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

## Affected Module

[[modules/<module-slug>/index]]

## Version

- **Target:** [[version/x.y.z]] or _backlog_

## Links

- Design: [[./design]] (or _pending_)
- Develop: [[./develop]] (or _pending_)
- Test: [[./test]] (or _pending_)
