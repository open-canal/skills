---
name: framework
description: Use when the user wants to initialize, scaffold, or update global project context for an Obsidian software specification vault.
license: MIT
compatibility: Agent Skills clients; no runtime required. Obsidian Skills (kepano/obsidian-skills) are optional and require Node, network, and git.
metadata:
  category: framework
  stage: setup
  workflow: init-update
---

# Framework

## Command Parameter

Use this skill with one command parameter:

- `init` — initialize the target specification vault.
- `update` — update global project description and framework-level vault context.

If the user does not provide a command, infer `init` when `.open-canal/` is missing and infer `update` when the user asks to revise product name, product description, or technical approach in an existing vault. Ask only if the command cannot be inferred.

## Load Standard

Read `references/standards/project-specification.md` first. Use bundled files from `references/standards/` and `references/templates/` for `init`, then install them into `.open-canal/standards/` and `.open-canal/templates/`.

For `update`, read the target vault's `.open-canal/standards/project-specification.md` when it exists; otherwise fall back to the bundled reference. The target vault's `.open-canal/standards/project-specification.md` and `.open-canal/standards/repository.md` become the runtime source of truth after initialization.

## Target Vault Root

All scaffold output goes to the **target project root** — the agent's current working directory (the user's project). Never create `.open-canal/`, `.obsidian/`, `modules/`, or `version/` inside the open-canal skill repository, a global skill directory, or an agent configuration directory.

Confirm the target root with the user only when it is ambiguous.

## Init Profile

For `init`, ask the standard's three required project profile questions (product name, product description, technical approach) before writing. For `update`, revise only the global project profile and derived summaries per the standard; do not rewrite requirement, version, design, develop, or test content unless the user explicitly asks.

## Obsidian Skills (Optional)

Probe whether obsidian-skills (`kepano/obsidian-skills`) are already available; do not install them automatically. Write a `## Required Skills` section into `.open-canal/AGENTS.md` listing the obsidian skills actually detected, or noting none were detected. When none are detected, suggest in the output report:

```bash
npx skills add https://github.com/kepano/obsidian-skills
```

## Workflow

### `init`

1. Determine the target vault root.
2. Ask the three project profile questions.
3. Probe obsidian-skills availability (do not install — see above).
4. Inspect the target repository; create missing shims, `.open-canal/`, `.obsidian/`, `modules/`, `version/`, and `version/0.1.0.md` per the standard. Do NOT create `DESIGN.md`.
5. Preserve user content in non-empty vaults; add only missing structure.
6. Run consistency checks from the standard; report failures and route future work to `demand`, `version`, `design`, `develop`, and `test`.

### `update`

1. Read `.open-canal/project.md`, `.open-canal/AGENTS.md`, and `.open-canal/development-standards/platform.md`.
2. Ask only for missing or changed project profile fields.
3. Update the global project profile and derived routing summary; preserve user-authored requirement, version, design, development, and test content.
4. Run the standard's strict migration: validate all file locations per skill standards, move misplaced files, and update all wikilinks.
5. Run consistency checks from the standard and report any manual migration items.

## Output

Return a structured report listing: command (`init` or `update`), target vault root, project profile fields changed, obsidian-skills availability (available / not detected), created files, changed files, manual migration items, consistency check failures, and the next recommended skill. If obsidian-skills were not detected during `init`, include the suggested install command.
