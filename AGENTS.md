# Agent Instructions

This repository is the open-canal methodology suite — a complete software specification workflow for coding agents, delivered as composable skills with shared standards, templates, and Skills CLI installation guidance. Install the suite once with `npx skills add`; agents trigger the relevant workflow skill on demand.

`skills/` is the canonical source for every skill package. `standards/` holds the detailed workflow rules; skill bodies route agents to standards and keep only minimal operating guidance. `templates/` holds document skeletons for all generated artifacts.

## Editing Rules

- Edit and create skills only under `skills/<skill-name>/`.
- Put detailed workflow rules in `standards/`; keep `SKILL.md` bodies as triggers, routing, and minimal operating guidance.
- Prefer `references/` for long background material. Do not add executable helpers; this repository is instruction-only.
- After any skill change, inspect the affected `SKILL.md`, `standards/`, and `CATALOG.md` entries before continuing.

## `SKILL.md` Review Rules

Check these manually before finishing:

- `name` in frontmatter must match the directory name exactly and match `^[a-z0-9][a-z0-9-]{0,62}[a-z0-9]?$`.
- `description` must start with `Use when` (warning).
- Body must stay under 500 lines (warning); move long detail to `references/`.
- Every `SKILL.md` must reference at least one `standards/` file (warning).
- `standards/INDEX.md` must list every skill; add new skills to it.

## Repository Commands

No build, install, or repository command is required. Agents read `skills/` and `standards/` directly.

## Current Skill Entrypoints

| Stage | Skill | Use |
| --- | --- | --- |
| Setup | `framework` | Run `init` to initialize an Obsidian software project specification vault, or `update` to revise global project context. |
| Product Plan | `demand` | Create or update a closed-loop requirement PRD in the owning module requirement pool. |
| Product Plan | `version` | Create `version/x.y.z.md`, add/remove requirements to/from an iteration version. |
| Implementation Plan | `design` | Create or update Stitch/Figma AI prototype prompts, manage the project DESIGN.md design system, and create requirement-local assets. |
| Implementation Plan | `develop` | Create or update client/service/database technical plan for downstream AI agents. |
| Implementation Plan | `test` | Create or update client/service test plans from a selected develop plan. |

## Standards

Use `standards/INDEX.md` to find the owning standard before changing a skill workflow. For repository-standard changes, follow `standards/standards-management.md`. See `CATALOG.md` for a skill overview.

## Compatibility Goal

Skills should stay portable across Claude Code, Codex CLI, Codex App, OpenCode, and Crush unless a skill explicitly documents a narrower requirement.
