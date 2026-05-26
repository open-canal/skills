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

1. **Derive the semver number** — apply the derivation rules from the standard. Always confirm the derived version with the user.
2. **Confirm release type** — confirm one of `major`, `minor`, `patch`, or `hotfix` per the standard.
3. **Create the version file** — create `version/x.y.z.md` using the version template and content requirements from the standard. Keep the empty `## Requirements` section from the template (the table header with the placeholder row) but do not add actual requirement rows — those are populated by `version add`.
4. **Validate scope** — report missing PRDs, ambiguous scope, and requirements too large for the version per the standard's scope rules.

### `add`

1. Confirm the target `version/x.y.z.md`.
2. Select one or more existing requirement PRDs.
3. Check duplicate membership, scope fit, and readiness gaps. Assess whether each requirement is small enough using the criteria in `.open-canal/standards/version.md`.
4. Update the version file and each affected PRD together.
5. Report missing design, develop, or test docs needed before implementation.

### `remove`

1. Read the version file and affected requirement PRDs.
2. Remove the requirement row from the version requirement list.
3. Update the requirement PRD's version status to `backlog`.
4. Update the version readiness checklist.
5. Preserve design, develop, and test docs — they remain valid for future version assignments.

## Output

### `create`

Return a structured report: version number, release type, version file path, goal, requirement count (or none), scope warnings, and missing PRDs referenced.

### `add`

Return the updated version file, added requirement links, skipped requirements, and readiness gaps.

### `remove`

Return the updated version file, removed requirement links, and affected PRDs with their new version status.
