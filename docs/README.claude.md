# open-canal for Claude Code

Complete guide for using the open-canal methodology suite with [Claude Code](https://claude.ai/code). Install the suite once; agents trigger the relevant workflow skill on demand.

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

Start Claude Code and load a skill:

```
Please load the demand skill
```

Claude Code discovers and loads the corresponding `SKILL.md` via its internal Skill tool.

## Discovery

Claude Code discovers skills from (in priority order):

1. Project-level: `.claude/skills/<name>/SKILL.md`
2. Global: `~/.claude/skills/<name>/SKILL.md`

The directory name is the skill identifier. The `name` field in `SKILL.md` frontmatter must match the directory name.

## Tool Mapping

| Generic Operation | Claude Code Tool |
| --- | --- |
| File I/O | `Read`, `Write`, `Edit` |
| Search | `Grep`, `Glob` |
| Shell | `Bash` |
| Sub-tasks | `Task` (via `description` and `subagent_type`) |
| Todo tracking | `TodoWrite` |

## Skill Entrypoints

| Stage | Skill | Commands |
| --- | --- | --- |
| Setup | `framework` | `init` initializes a vault; `update` revises global project context |
| Product Plan | `demand` | `create` creates a new requirement PRD; `update` revises requirement details |
| Product Plan | `version` | `create` creates a version iteration; `add` assigns requirements; `remove` removes requirements |
| Implementation Plan | `design` | `create` generates Stitch/Figma prototype prompts; `update` revises after upstream changes |
| Implementation Plan | `develop` | `create` generates client/service/database technical plans; `update` revises after upstream changes |
| Implementation Plan | `test` | `create` generates client/service/cross-end test plans; `update` revises after upstream changes |

## Troubleshooting

### Skills not found

1. Re-run `npx skills add https://github.com/open-canal/skills`.
2. Verify `SKILL.md` frontmatter `name` matches the directory name.
3. Restart the Claude Code process.

### Skills don't follow rules after loading

1. Reinstall with `npx skills add https://github.com/open-canal/skills`.
2. Restart Claude Code so it reloads the installed skill metadata.
3. Open an issue with the skill name, agent version, and failing workflow if the behavior still diverges.
