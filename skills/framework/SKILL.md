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

## Project Profile Questions

For `init`, ask exactly these required questions before writing the scaffold:

1. Product name: what is the product called?
2. Product description: what does the product do?
3. Technical approach: what broad platform or architecture is planned (web app, iOS app, desktop app, API service, etc.)? Include concrete frameworks when known.

Use the answers to initialize `.open-canal/project.md`, the project summary in `.open-canal/AGENTS.md`, and `.open-canal/development-standards/platform.md`.

For `update`, use the same fields to update global framework-level context. Do not rewrite requirement PRDs, design prompts, development plans, test plans, or version scope unless the user explicitly asks.

## Version Bootstrap

During `init`, create `version/0.1.0.md` when no `version/x.y.z.md` files exist. Use the version template as a planning placeholder. Do not ask version-specific questions during `init`, and do not copy the product description or initialization summary into the `0.1.0` version goal, release notes, or requirement list.

## Obsidian Skills (Optional)

Obsidian Skills (`kepano/obsidian-skills`) give agents specialized knowledge of Obsidian Flavored Markdown, Bases, JSON Canvas, and the Obsidian CLI. They improve output quality when generating Obsidian content.

**Check availability — do not install automatically.** Probe whether obsidian-skills are already available to the agent (e.g., list installed skills, check the skills directory).

- **Already available:** probe the installed skills. Dynamically write a `## Required Skills` section into `.open-canal/AGENTS.md` listing every actually detected obsidian skill name and description. State that downstream skills should invoke the relevant obsidian skill when generating Obsidian content.
- **Not available:** still write the `## Required Skills` section with a note that obsidian-skills were not detected. In the output report, suggest the user run:

  ```bash
  npx skills add https://github.com/kepano/obsidian-skills
  ```

  But do NOT execute this command during initialization. Installing skills produces agent-level side effects that are separate from vault initialization.

Do NOT hardcode the skill list — it must reflect what was actually detected.

## Workflow

### `init`

1. Determine the target vault root.
2. Ask the three project profile questions.
3. Check obsidian-skills availability (do not install — see above).
4. Inspect the target repository (empty or existing vault).
5. Create missing root shims, `.open-canal/`, `.open-canal/project.md`, `.open-canal/standards/`, `.open-canal/templates/`, `.obsidian/`, `modules/`, `version/`, and `version/0.1.0.md` per the standard. Do NOT create `DESIGN.md`.
6. For non-empty vaults, preserve user content and add only missing structure.
7. Run consistency checks from the standard. Report failures.
8. Route future work to `demand`, `version`, `design`, `develop`, and `test`.

### `update`

1. Read `.open-canal/project.md`, `.open-canal/AGENTS.md`, and `.open-canal/development-standards/platform.md`.
2. Ask only for missing or changed project profile fields.
3. Update the global project profile and derived routing summary.
4. Preserve user-authored requirement, version, design, development, and test content.
5. Run consistency checks from the standard and report any manual migration items.

## Output

Return a structured report listing: command (`init` or `update`), target vault root, project profile fields changed, obsidian-skills availability (available / not detected), created files, changed files, manual migration items, consistency check failures, and the next recommended skill. If obsidian-skills were not detected during `init`, include the suggested install command.
