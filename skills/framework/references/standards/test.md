# Test Standard

## Test

Use `test` with `create` to generate a test plan from a specific `develop.md`. Use `test` with `update` to update an existing test plan after PRD, design, develop, or version changes.

The test workflow must always target a specific `develop.md` as its primary source. PRD, design, and version scope are derived from develop.md links — never ask the user for a target iteration. When the develop plan includes both client and service work, the test plan must separately cover client tests, service tests, and cross-end contract tests mapped to develop.md contracts.

The test artifact belongs in the same requirement pool:

```text
modules/<module-slug>/requirements/<requirement-slug>/test.md
```

The output should be suitable for downstream client and service AI agents to execute tests in their own code repositories.

## Inputs

Read:

- requirement `develop.md` — **primary source**; derive PRD, design, and version links from it;
- requirement `prd.md` (from develop.md link);
- requirement `design.md` when UI behavior exists (from develop.md link);
- `version/x.y.z.md` when referenced by develop.md or PRD (derived, not asked);
- previous `test.md` when updating.

### Cross-End Contract Dependency

Cross-end contract tests depend on the Contracts section in `develop.md`. If contracts are not defined:

1. Report the missing contracts as a blocker for cross-end tests.
2. Generate client and service test plans for everything else.
3. Insert placeholder rows for contract tests with the note: "Blocked — contracts not defined in develop.md."

## Test Plan Requirements

Use `.open-canal/templates/test.md` as the document skeleton. `test.md` must include:

- **Primary Develop** link — `[[./develop]]` (required; the source develop plan);
- PRD link — `[[./prd]]` (derived from develop.md);
- design link when UI behavior exists — `[[./design]]` (derived from develop.md);
- version link when referenced by develop.md or PRD — `[[version/x.y.z]]` (derived, not asked);
- client test plan (when develop.md includes client tasks);
- service test plan (when develop.md includes service tasks);
- cross-end contract tests (when contracts exist in develop.md; otherwise blocked or N/A);
- acceptance scenarios mapped to PRD flow;
- permission, error, empty, loading, cancellation, and recovery cases;
- offline/realtime/idempotency cases when relevant;
- required fixtures or seed data;
- execution notes for downstream AI agents;
- evidence locations under requirement-local `assets/test/` or version-level notes;
- blocked cases with reasons.

### Test Case Format

Every test case uses **Given-When-Then** format:

| ID | Scenario | Given | When | Then |
|----|----------|-------|------|------|

- **Given:** preconditions — DB state, auth state, UI state, mock data.
- **When:** the specific action or trigger — API call, button click, navigation, event.
- **Then:** the expected observable outcome — status code, UI change, data written, event emitted.

Test case IDs follow the pattern `{C|S|X}-{group}{sequence}`:
- `C` = Client test, `S` = Service test, `X` = Cross-end test.
- Client groups: `01` = UI states, `N1` = Navigation, `O1` = Offline, `A1` = Accessibility.
- Service groups: `C01` = Contract, `AU1` = Auth, `P1` = Persistence, `E1` = Error, `R1` = Realtime.
- Cross-end groups: `01` = Contract integration.

## Client Tests

Cover UI states, navigation, permissions, local cache behavior, offline behavior, accessibility checks, and design conformance when relevant.

## Service Tests

Cover API contracts, auth and authorization, persistence, idempotency, realtime events, error responses, and migration compatibility when relevant.

## Update Rules

When PRD, design, develop, or version scope changes:

1. Compare the changed artifact (PRD/design/develop) against the existing test plan.
2. A test scenario is affected when: the flow step it tests was modified, the contract it validates changed signature, the user-facing behavior it asserts no longer matches the PRD, or the acceptance criteria it maps to was removed or rewritten.
3. Update affected test scenarios.
4. Remove tests for behavior no longer in scope.
5. Mark missing implementation details as blockers rather than inventing contracts.
6. Keep evidence locations tied to the requirement or version.
