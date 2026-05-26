---
name: design
description: Use when the user wants Stitch or Figma AI prototype prompts, requirement-local design assets, or a DESIGN.md design system for a specific demand PRD.
license: MIT
compatibility: Agent Skills clients; requires access to the target Obsidian specification vault and target prototype tool context.
metadata:
  category: design
  stage: implementation-plan
  workflow: create-update
---

# Design

## Command Parameter

Use this skill with one command parameter:

- `create` — create a new design artifact and prototype prompt for a demand PRD.
- `update` — update an existing design artifact after PRD or DESIGN.md changes.

If the user does not provide a command, ask which to run rather than guessing. The design workflow must always target a specific demand PRD.

## Load Standard

Read target-vault `.open-canal/standards/design.md` (`DESIGN.md Maintenance` and `Design` sections) and `.open-canal/standards/repository.md` (assets placement). If `.open-canal/standards/` is missing, stop and ask the user to run `framework init` first.

## Load Template

Use `.open-canal/templates/design.md` as the document skeleton.

## Workflow

### Common: DESIGN.md Gate

Before generating any prototype prompt, check `.open-canal/design-standards/DESIGN.md` per the standard's DESIGN.md creation/update workflow:

- **Missing** → follow the standard. The standard defines when to pause (all four skeletons empty + no user references) vs when to synthesize.
- **Incomplete** → report gaps per the standard (tool-dependent: §§1–5 always; §6 Stitch-only).
- **Complete** → proceed.

### `create`

1. Read the owning demand `prd.md`. Confirm which module and requirement the design targets.
2. Run the DESIGN.md gate (above). Do not proceed with prototype prompts until DESIGN.md is approved.
3. Verify PRD closed-loop elements are present per the standard's ambiguity check.
4. Generate `modules/<module-slug>/requirements/<requirement-slug>/design.md` from the template.
5. Build the prototype prompt using DESIGN.md §§1–5 for all tools; §6 only for Stitch. Create requirement-local assets folders per the standard.
6. Update the source PRD's design link to point to the new `design.md`.
7. Report stale assets, DESIGN.md gaps, PRD ambiguity. Mark downstream docs affected.

### `update`

1. Compare old and new PRD behavior. List every user-visible change before editing the design.
2. Check DESIGN.md for staleness — if new PRD behavior requires design tokens not in DESIGN.md, report the gap.
3. Update only affected screens, states, and prompt sections in `design.md`.
4. Keep previous prototype asset links when still valid. Mark stale assets with file path and reason.
5. Update the source PRD's design link if it changed.
6. Report the stale asset list alongside the updated design artifact.

## Output

Return the design artifact path, prototype prompt summary, stale asset list, DESIGN.md reference, and gaps.
