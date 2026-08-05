---
name: test
description: Use when the user wants client, service, contract, QA, or acceptance test plans for a requirement PRD, version iteration, design, or technical plan.
license: MIT
compatibility: Agent Skills clients; requires access to the target Obsidian specification vault and downstream repository context when available.
metadata:
  category: test
  stage: implementation-plan
  workflow: create-update
---

# Test

## Command Parameter

Use this skill with one command parameter:

- `create` — create a new test plan from a target `develop.md`.
- `update` — update an existing test plan after PRD, design, develop, or version changes.

If the user does not provide a command, ask which to run rather than guessing. The test workflow must always target a specific `develop.md` as its primary source.

## Load Standard

Read target-vault `.open-canal/standards/test.md`. Also read `.open-canal/standards/workflow.md` for change propagation rules. If `.open-canal/standards/` is missing, stop and ask the user to run `framework init` first.

## Load Template

Use `.open-canal/templates/test.md` as the document skeleton for all generated test plans.

## Workflow

### `create`

1. Confirm the target `develop.md` as the primary source. The test plan derives its scope from the develop plan.
2. Read the requirement PRD, linked design, and develop docs. Derive version from links.
3. Read the target vault's `.open-canal/development-standards/testing.md` and relevant platform/client/service/database standards. Apply testing framework, fixture, and environment conventions that constrain the test plan.
4. Generate `modules/<module-slug>/requirements/<requirement-slug>/test.md` from the template per the standard: client/service sections only where the develop plan has those ends; cross-end contract tests blocked when contracts are missing (or N/A for single-end plans); use the standard's Given-When-Then format and ID scheme.
5. Report missing implementation details as blockers instead of inventing contracts.
6. Update the source PRD's test link to point to the new `test.md`.

### `update`

1. Compare the changed artifact (PRD/design/develop) against the existing test plan.
2. A test scenario is affected when: the flow step it tests was modified, the contract it validates changed signature, the user-facing behavior it asserts no longer matches the PRD, or the acceptance criteria it maps to was removed or rewritten.
3. Update affected test scenarios in `test.md`.
4. Remove tests for behavior no longer in scope.
5. Mark missing implementation details as blockers rather than inventing contracts.
6. Keep evidence locations tied to the requirement or version.

## Output

Return the test artifact path, client test plan, service test plan, cross-end test plan (or blocked status), blocked cases, and evidence locations.
