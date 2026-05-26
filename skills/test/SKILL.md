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
4. Generate `modules/<module-slug>/requirements/<requirement-slug>/test.md` using the test template.
5. Generate test sections only for the ends present in the target `develop.md`:
   - **Client Tests** — generated when develop.md includes client design or client tasks; covers UI states, navigation, offline, accessibility.
   - **Service Tests** — generated when develop.md includes service design or service tasks; covers contracts, auth, persistence, error, realtime.
   - **Cross-End Contract Tests** — generated only when contracts exist in develop.md; if develop.md includes both client and service work but no contracts, mark cross-end tests as blocked. If develop.md is client-only or service-only, mark cross-end as N/A.
6. All test cases use Given-When-Then format with the standard ID scheme (`C-`, `S-`, `X-` prefixes).
7. Generate only the applicable client/service tests from the target develop.md. Cross-end contract tests are blocked only when both client and service ends exist in develop.md but contracts are missing. Do not generate client tests for service-only plans, and do not generate service tests for client-only plans.
8. Report missing implementation details as blockers instead of inventing contracts.
9. Update the source PRD's test link to point to the new `test.md`.

### `update`

1. Compare the changed artifact (PRD/design/develop) against the existing test plan.
2. A test scenario is affected when: the flow step it tests was modified, the contract it validates changed signature, the user-facing behavior it asserts no longer matches the PRD, or the acceptance criteria it maps to was removed or rewritten.
3. Update affected test scenarios in `test.md`.
4. Remove tests for behavior no longer in scope.
5. Mark missing implementation details as blockers rather than inventing contracts.
6. Keep evidence locations tied to the requirement or version.

## Output

Return the test artifact path, client test plan, service test plan, cross-end test plan (or blocked status), blocked cases, and evidence locations.
