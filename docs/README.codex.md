# open-canal for Codex

Complete guide for using the open-canal methodology suite with [Codex CLI](https://github.com/openai/codex) or Codex App. Install the suite once; agents trigger the relevant workflow skill on demand.

## Installation

Install with the Skills CLI:

```bash
npx skills add https://github.com/open-canal/skills
```

To install a specific entrypoint:

```bash
npx skills add https://github.com/open-canal/skills --skill framework
```

The `npx skills add` path is the supported public installation path and contributes anonymous install telemetry to skills.sh when telemetry is enabled. After installation, run `framework init` from the target project root; it captures product name, product description, and technical approach, then creates `.open-canal/standards/` and `.open-canal/templates/`.

## Verification

Start Codex CLI and check that skills are discovered:

```
list available skills
```

Or load directly:

```
use the demand skill to create a new requirement
```

## Discovery

Codex discovers skills from:

1. Project-level: `.agents/skills/<name>/SKILL.md`
2. Global: `~/.agents/skills/<name>/SKILL.md`

The directory name is the skill identifier. The `name` field in `SKILL.md` frontmatter must match the directory name.

## Tool Mapping

| Generic Operation | Codex Tool |
| --- | --- |
| File I/O | Native file tools |
| Search | `grep`, `glob` |
| Shell | `bash` / `shell` |
| Sub-tasks | Task agent / subagent |
| Todo tracking | TodoWrite / task tracking |

## Skill Entrypoints

| Stage | Skill | Commands |
| --- | --- | --- |
| Setup | `framework` | `init` initializes a vault; `update` revises global project context |
| Product Plan | `demand` | `create` creates a new requirement PRD; `update` revises requirement details |
| Product Plan | `version` | `create` creates a version iteration; `add` assigns requirements; `remove` removes requirements |
| Implementation Plan | `design` | `create` generates Stitch/Figma prototype prompts; `update` revises after upstream changes |
| Implementation Plan | `develop` | `create` generates client/service/database technical plans; `update` revises after upstream changes |
| Implementation Plan | `test` | `create` generates client/service/cross-end test plans; `update` revises after upstream changes |

## Notes

1. Codex App and CLI use the same discovery mechanism (`.agents/skills/` path).
2. Codex loads skill content on demand; skills only inject context when triggered.
3. Globally installed skills are available in all projects.
4. Re-run `npx skills add https://github.com/open-canal/skills` to refresh installed skills.

## Troubleshooting

### Skills not found

1. Re-run `npx skills add https://github.com/open-canal/skills`.
2. Confirm Codex is launched from the correct directory for project-specific work.
3. Check `SKILL.md` frontmatter format (`name` + `description` required).

### Skills behave unexpectedly

1. Reinstall with `npx skills add https://github.com/open-canal/skills`.
2. Restart Codex so it reloads the installed skill metadata.
3. Open an issue with the skill name, agent version, and failing workflow if the behavior still diverges.
