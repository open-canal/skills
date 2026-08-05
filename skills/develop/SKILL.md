---
name: develop
description: Use when the user wants client, service, database, API, or contract technical plans for downstream AI coding agents from a demand PRD or design artifact.
license: MIT
compatibility: Agent Skills clients; requires access to the target Obsidian specification vault and downstream repository context when available.
metadata:
  category: develop
  stage: implementation-plan
  workflow: create-update
---

# Develop

## Command Parameter

Use this skill with one command parameter:

- `create` — create a new technical plan for a demand PRD.
- `update` — update an existing technical plan after PRD or design changes.

If the user does not provide a command, ask which to run rather than guessing. The develop workflow must always target a specific demand PRD.

## Load Standard

Read target-vault `.open-canal/standards/development.md`, especially `Develop`. Also read `.open-canal/standards/workflow.md` for parallel execution rules with `design`. If `.open-canal/standards/` is missing, stop and ask the user to run `framework init` first.

## Load Template

Use `.open-canal/templates/develop.md` as the document skeleton for all generated development plans.

## Workflow

### `create`

1. Select and read the demand `prd.md`.
2. Read `design.md` when UI or interaction behavior exists.
3. Read the target vault's `.open-canal/development-standards/` files (platform, client, service, database, testing). Apply all conventions that constrain the development plan.
4. Read linked version and existing `develop.md` when present.
5. Generate or update `modules/<module-slug>/requirements/<requirement-slug>/develop.md`. When both client and service are involved, include separate Client Design/Tasks and Service Design/Tasks sections plus a Contracts section per the standard.
6. Keep downstream AI agent tasks small, explicit, independent, and traceable to PRD/design decisions. Follow task granularity rules in `.open-canal/standards/development.md`.
7. Group tasks for independent parallel execution.
8. Update the source PRD's develop link to point to the new `develop.md`.

### `update`

1. Compare changed user-facing behavior before editing technical tasks.
2. Update affected client/service/database sections together in `develop.md`.
3. Re-validate contracts against the updated PRD/design.
4. Keep downstream agent tasks small enough to execute independently.
5. Report any stale task, contract, or data model that conflicts with the latest PRD/design.

## Output

Return the development artifact path, generated client/service/database task groups, cross-end contracts, blockers, and downstream agent handoff notes.
