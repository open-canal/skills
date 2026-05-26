# open-canal for Crush

Complete guide for using the open-canal methodology suite with [Crush](https://crush.ai). Install the suite once; agents trigger the relevant workflow skill on demand.

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

Start Crush and load a skill:

```
load the demand skill
```

Crush discovers and loads the corresponding `SKILL.md` via the Agent Skills open standard.

## Discovery

Crush discovers skills from (in priority order):

1. `.crush/skills/<name>/SKILL.md` (native)
2. `.agents/skills/<name>/SKILL.md` (Agent Skills standard)
3. `.claude/skills/<name>/SKILL.md` (Claude compatibility)
4. Corresponding global paths: `~/.crush/skills/`, `~/.agents/skills/`, `~/.claude/skills/`

Crush supports multiple discovery paths. The Skills CLI installs into the appropriate supported path for the selected agent.

## Tool Mapping

| Generic Operation | Crush Tool |
| --- | --- |
| File I/O | `read_file`, `write_file`, `edit_file` |
| Search | `grep`, `glob` |
| Shell | `shell` |
| Sub-tasks | subagent / task |
| Todo tracking | task tracking |

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

1. Crush's multi-path discovery may cause the same skill to appear multiple times; this is usually harmless. Remove redundant installed copies if duplicates appear.
2. Crush's Agent Skills implementation is based on the open standard and is fully compatible with Claude Code and Codex skill formats.
3. `SKILL.md` frontmatter must include `name` and `description` in standard format.
4. Re-run `npx skills add https://github.com/open-canal/skills` to refresh installed skills.

## Troubleshooting

### Skills not found

1. Re-run `npx skills add https://github.com/open-canal/skills`.
2. Confirm Crush is launched from the correct project directory.
3. Check `SKILL.md` frontmatter includes `name` and `description`.

### Duplicate skills

1. Crush may discover the same skill from multiple paths (`.crush/`, `.agents/`, `.claude/`).
2. To deduplicate, keep only one installed copy of each open-canal skill.
3. Remove stale duplicate copies from older installation attempts.

### Skills can't read standards

1. Reinstall with `npx skills add https://github.com/open-canal/skills`.
2. Restart Crush so it reloads the installed skill metadata.
3. Open an issue with the skill name, agent version, and failing workflow if the behavior still diverges.
