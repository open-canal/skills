# open-canal Agent Integration Guide

open-canal is a software specification skill suite: `skills/` is the entrypoint, `standards/` and `templates/` are shared authoring infrastructure. Install the suite once with the Skills CLI; agents trigger the relevant workflow skill on demand.

## Installation

```bash
npx skills add https://github.com/open-canal/skills
```

To install a single entrypoint:

```bash
npx skills add https://github.com/open-canal/skills --skill framework
```

The Skills CLI installs into the selected agent's discovery path (`.claude/skills/`, `.agents/skills/`, `.opencode/skills/`, or `.crush/skills/`). Installing makes skills discoverable but does not create `.open-canal/` scaffolding — run `framework init` from the target project root to create `.open-canal/standards/` and `.open-canal/templates/`.

## Agent Notes

### Codex CLI / App

- **Discovery:** project `.agents/skills/<name>/SKILL.md`; global `~/.agents/skills/<name>/SKILL.md`. The directory name and frontmatter `name` must match.
- **Verify:** ask Codex to list available skills, or load one directly (e.g. "use the demand skill to create a new requirement").
- **Troubleshoot:** re-run `npx skills add ...`, confirm the launch directory, check frontmatter, and restart Codex.

### Claude Code

- **Discovery:** project `.claude/skills/`; global `~/.claude/skills/`.
- **Verify:** "Please load the demand skill."
- **Troubleshoot:** reinstall, verify frontmatter `name` matches the directory name, and restart.

### OpenCode

- **Discovery:** `.opencode/skills/` and `~/.config/opencode/skills/`, plus `.claude/skills/` and `.agents/skills/` compatibility paths.
- **Verify:** "List available skills."
- **Permissions:** allow skills via `permission.skill` in `opencode.json`.
- **Troubleshoot:** reinstall, check the working directory, check permissions, and restart.

### Crush

- **Discovery (priority):** `.crush/skills/` → `.agents/skills/` → `.claude/skills/`, with matching global paths.
- **Verify:** "load the demand skill."
- **Duplicates:** Crush may discover the same skill from multiple paths; keep only one installed copy.
- **Troubleshoot:** reinstall, check frontmatter, and restart.

## Refreshing Installed Skills

Re-run `npx skills add https://github.com/open-canal/skills`, then restart the agent so it reloads the skill metadata.
