# open-canal

[![skills.sh](https://skills.sh/b/open-canal/skills)](https://skills.sh/open-canal/skills)

open-canal is a complete AI software specification methodology for coding agents, delivered as a suite of composable skills, shared standards, and templates.

Install the suite once; agents trigger the relevant workflow skill on demand:

Setup:

- `framework` — initialize a specification vault with `init`, or update global project context with `update`.
- `xcode-mcp` — configure, verify, troubleshoot, or remove Xcode MCP access for the current AI agent.

Product plan:

- `demand` — turn feature ideas into closed-loop PRDs with `create` or `update`.
- `version` — scope releases with `create`, assign requirements with `add`, or remove with `remove`.

Implementation plan:

- `design` — generate Stitch/Figma AI prototype prompts with `create` or `update`, manage the project DESIGN.md design system.
- `develop` — produce client, service, and database technical plans with `create` or `update`.
- `test` — generate client, service, and cross-end contract test plans with `create` or `update`.

These skills follow the [Agent Skills specification](https://agentskills.io/specification) so they can be used by any skills-compatible agent, including Claude Code, Codex CLI, OpenCode, and Crush.

## Installation

Install open-canal with the Skills CLI:

```bash
npx skills add https://github.com/open-canal/skills
```

To install a specific entrypoint:

```bash
npx skills add https://github.com/open-canal/skills --skill framework
```

The `npx skills add` path is the public installation path. It makes open-canal discoverable by compatible agents and contributes anonymous install telemetry to skills.sh when telemetry is enabled.

Installing open-canal does not create a `.open-canal/` scaffold in any project. To initialize a specification vault, run `framework init` from the target project root directory; it asks for product name, product description, and technical approach, then creates `.open-canal/standards/` and `.open-canal/templates/` in that project.

### Agent-Specific Notes

See [docs/README.md](docs/README.md) for per-agent installation, discovery, verification, and troubleshooting notes for Claude Code, Codex CLI/App, Crush, and OpenCode.

## Skills

| Stage | Skill | Description |
|-------|-------|-------------|
| Setup | [framework](skills/framework) | Run `init` to initialize a vault, or `update` to revise global product context |
| Setup | [xcode-mcp](skills/xcode-mcp) | Configure, verify, troubleshoot, or remove Xcode MCP access for the current AI agent |
| Product Plan | [demand](skills/demand) | Create or update a closed-loop requirement PRD in the owning module requirement pool |
| Product Plan | [version](skills/version) | Create a version iteration file (`version/x.y.z.md`), add or remove requirements |
| Implementation Plan | [design](skills/design) | Create or update Stitch/Figma AI prototype prompts from a demand PRD, manage the project DESIGN.md design system, and create requirement-local assets |
| Implementation Plan | [develop](skills/develop) | Create or update client, service, and database technical plans for downstream AI development agents |
| Implementation Plan | [test](skills/test) | Create or update client, service, and cross-end contract test plans from a selected develop plan |

## Authoring

- `skills/` is the entrypoint source for every workflow skill. `standards/` and `templates/` are shared infrastructure — the three form a single suite.
- After `framework init`, runtime standards and templates live in the target project under `.open-canal/standards/` and `.open-canal/templates/`.
- Put detailed workflow rules in `standards/`; keep `SKILL.md` bodies as triggers, routing, and minimal operating guidance.
- Prefer `references/` for long background material. Do not add executable helpers; this repository is instruction-only.
- Each `SKILL.md` must reference at least one `standards/` file.
- `name` in frontmatter must match the directory name exactly and match `^[a-z0-9][a-z0-9-]{0,62}[a-z0-9]?$`.
