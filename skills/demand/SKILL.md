---
name: demand
description: Use when the user wants to create, write, split, or update a closed-loop requirement PRD for a feature in an Obsidian software specification vault.
license: MIT
compatibility: Agent Skills clients; requires access to the target Obsidian specification vault.
metadata:
  category: demand
  stage: product-plan
  workflow: create-update
---

# Demand

## Command Parameter

Use this skill with one command parameter:

- `create` — create a new requirement PRD.
- `update` — update an existing requirement PRD.

If the user does not provide a command, inspect existing PRDs in the target vault and propose either `create` or `update` with a brief rationale. Ask the user to confirm before writing.

## Load Standard

Read target-vault `.open-canal/standards/demand.md` and `.open-canal/standards/repository.md`. If `.open-canal/standards/` is missing, stop and ask the user to run `framework init` first.

## Load Template

Use `.open-canal/templates/prd.md` as the document skeleton.

## Workflow

### `create`

1. Inspect the target vault's `.open-canal/AGENTS.md`, `modules/index.md`, and relevant module indexes.
2. Help the user narrow the request into one closed-loop requirement point (criteria in `.open-canal/standards/demand.md`).
3. Run the confirmation gate — confirm the requirement boundary before writing. Skip only when the user explicitly gave enough detail.
4. Generate `modules/<module-slug>/requirements/<requirement-slug>/prd.md` from the template.
5. Run the completeness checklist from `.open-canal/standards/demand.md` before confirming.
6. Update `modules/index.md`, the module index, and affected version files. Report downstream docs needing refresh.

### `update`

1. Read the existing PRD, linked design, develop, test, and version files.
2. Preserve accepted scope unless the user asks to change it.
3. Before writing changes, produce a change summary listing: what sections changed, what behavior was added/removed/modified, and why.
4. Update `modules/<module-slug>/requirements/<requirement-slug>/prd.md`.
5. Update downstream links or mark affected downstream docs as needing refresh.
6. Report any design, development, version, or test file that now conflicts with the PRD.

## Output

Return the PRD path, confirmation status, changed indexes, unresolved product questions, and downstream follow-up skills.
