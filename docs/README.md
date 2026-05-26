# open-canal Integration Guide

open-canal is a complete software specification and development collaboration methodology, delivered as a skill suite. `skills/` is the entrypoint; `standards/` and `templates/` are shared authoring infrastructure. Install the suite once with the Skills CLI; agents trigger the relevant workflow skill on demand. `skills/` is the authoritative entrypoint source.

## Supported AI Coding Agents

| Agent | Integration Doc | Discovery Path |
| --- | --- | --- |
| OpenCode | [README.opencode.md](README.opencode.md) | `.opencode/skills/` |
| Claude Code | [README.claude.md](README.claude.md) | `.claude/skills/` |
| Codex CLI / App | [README.codex.md](README.codex.md) | `.agents/skills/` |
| Crush | [README.crush.md](README.crush.md) | `.crush/skills/`, `.agents/skills/`, `.claude/skills/` |

## Quick Start

```bash
npx skills add https://github.com/open-canal/skills
```

The `npx skills add` path is the supported public installation path. It makes open-canal discoverable by compatible agents and contributes anonymous install telemetry to skills.sh when telemetry is enabled. After installation, run `framework init` from the target project root; it captures product name, product description, and technical approach, then creates `.open-canal/standards/` and `.open-canal/templates/`.

See the integration docs above for agent-specific verification and usage notes.

## Core Principles

1. `skills/` is the authoritative entrypoint; `standards/` and `templates/` are shared authoring infrastructure for the suite.
2. After `framework init`, runtime standards and templates live in the target project under `.open-canal/standards/` and `.open-canal/templates/`.
3. Detailed workflow rules for each skill live in `standards/`; the skill body handles routing and triggering only.
4. Skills remain portable across agents unless a skill explicitly declares a narrower compatibility range.
5. Installing the open-canal suite makes skills discoverable but does not create `.open-canal/` scaffolding. Run `framework init` to initialize a specification vault.

## Skill Entrypoints

| Stage | Skill | Commands |
| --- | --- | --- |
| Setup | `framework` | `init` initializes a vault; `update` revises global project context |
| Product Plan | `demand` | `create` creates a new requirement PRD; `update` revises requirement details |
| Product Plan | `version` | `create` creates a version iteration; `add` assigns requirements; `remove` removes requirements |
| Implementation Plan | `design` | `create` generates Stitch/Figma prototype prompts; `update` revises after upstream changes |
| Implementation Plan | `develop` | `create` generates client/service/database technical plans; `update` revises after upstream changes |
| Implementation Plan | `test` | `create` generates client/service/cross-end test plans; `update` revises after upstream changes |
