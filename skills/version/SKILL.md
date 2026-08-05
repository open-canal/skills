---
name: version
description: Use when the user wants to create a version plan, release iteration, semver milestone, or add/remove existing PRD requirements in a version.
license: MIT
compatibility: Agent Skills clients; requires access to the target Obsidian specification vault.
metadata:
  category: version
  stage: product-plan
  workflow: create-add-remove
---

# Version

## Command Parameter

Use this skill with one command parameter:

- `create` — create a new `version/x.y.z.md` version iteration file.
- `add` — add one or more existing requirement PRDs to a version.
- `remove` — remove one or more requirements from a version.

If the user does not provide a command, infer `create` / `add` / `remove` only when the user's intent is unambiguous (e.g. "make a new version" → create; "put login into v1.2.0" → add; "take checkout out of the version" → remove). Otherwise ask the user which command to run.

## Load Standard

Read target-vault `.open-canal/standards/version.md`. Also read `.open-canal/standards/demand.md` when a requirement is too broad and needs PRD splitting. If `.open-canal/standards/` is missing, stop and ask the user to run `framework init` first.

## Load Template

Use `.open-canal/templates/version.md` as the document skeleton.

## Workflow

### `create`

1. **Derive and confirm the semver number** with the user per the standard's derivation rules.
2. **Confirm release type** — one of `major`, `minor`, `patch`, or `hotfix` per the standard.
3. **Create the version file** — `version/x.y.z.md` from the template per the standard's content requirements (keep the empty `## Requirements` section; actual rows are populated by `version add`).
4. **Validate scope** — report missing PRDs, ambiguous scope, and requirements too large per the standard's scope rules.

### `add`

1. Confirm the target `version/x.y.z.md`.
2. Select one or more existing requirement PRDs.
3. Check duplicate membership, scope fit, and readiness gaps per the standard.
4. Update the version file and each affected PRD together.
5. Report missing design, develop, or test docs needed before implementation.

### `remove`

1. Read the version file and affected PRDs; remove the requirement row, set the PRD status to `backlog`, and update the readiness checklist.
2. Preserve design, develop, and test docs — they remain valid for future version assignments.

## Output

Return a structured report: version number, release type, version file path, goal, requirement count (or none), scope warnings, and missing PRDs for `create`; updated version file, added/skipped requirement links, and readiness gaps for `add`; updated version file, removed requirement links, and affected PRD statuses for `remove`.
