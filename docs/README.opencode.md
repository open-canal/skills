# open-canal for OpenCode

Complete guide for using the open-canal methodology suite with [OpenCode](https://opencode.ai). Install the suite once; agents trigger the relevant workflow skill on demand.

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

Start OpenCode and ask:

```
List available skills
```

The AI should list all open-canal skill entrypoints.

Or load and test directly:

```
Load the demand skill and help me create a new requirement
```

## Discovery

OpenCode discovers skills from:

- Project-level: `.opencode/skills/<name>/SKILL.md`
- Global: `~/.config/opencode/skills/<name>/SKILL.md`
- Claude compatibility: `.claude/skills/<name>/SKILL.md`
- Agent compatibility: `.agents/skills/<name>/SKILL.md`

OpenCode supports multiple discovery paths. The Skills CLI installs into the appropriate supported path for the selected agent.

## Permissions

To restrict specific skills, configure in `opencode.json`:

```json
{
  "permission": {
    "skill": {
      "*": "allow",
      "framework": "allow",
      "internal-*": "deny"
    }
  }
}
```

## Relationship with Superpowers

open-canal's design references [Superpowers](https://github.com/obra/superpowers)' cross-agent skill suite pattern, but focuses on **software specification workflows** (product planning: demand and version; implementation planning: design, development, and testing) rather than general-purpose development helpers.

The two can coexist; SKILL.md formats are compatible.

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
2. Confirm the current directory is your target project root for specification work.
3. Check permissions in `opencode.json`.
4. Restart OpenCode.

### Skills behave unexpectedly after loading

1. Reinstall with `npx skills add https://github.com/open-canal/skills`.
2. Restart OpenCode so it reloads the installed skill metadata.
3. Open an issue with the skill name, agent version, and failing workflow if the behavior still diverges.
