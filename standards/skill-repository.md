# Skill Repository Standard

## Canonical Layout

`skills/` is the canonical source for every skill package. There are no agent-specific skill folders; agents load skills directly from `skills/`.

```text
skills/<skill-name>/SKILL.md
standards/
templates/
```

## Skill Package Rules

- Every skill has `skills/<skill-name>/SKILL.md`.
- `name` matches `<skill-name>` exactly and uses lowercase letters, numbers, and hyphens.
- `description` starts with `Use when...` and describes triggering conditions only.
- `SKILL.md` should route to standards and essential resources; detailed rules live in `standards/` or `references/`.
- Use `references/` inside a skill only for large supporting material that should load on demand.
- Do not add executable helpers. Skills should instruct agents which files to read, create, update, and inspect.
- Do not add README, changelog, or user-facing documentation inside individual skill folders.

## Agent Compatibility

Skills should remain portable across Claude Code, Codex CLI, Codex App, OpenCode, and Crush unless a skill explicitly documents a narrower requirement.

Install open-canal as a skill group through the Skills CLI. Public user-facing docs must use `npx skills add https://github.com/open-canal/skills` so installations are visible to skills.sh telemetry when telemetry is enabled.

### Public Installation

Use the Skills CLI:

```bash
npx skills add https://github.com/open-canal/skills
```

For a single entrypoint:

```bash
npx skills add https://github.com/open-canal/skills --skill framework
```

Do not publish non-CLI installation paths for users. They bypass skills.sh install tracking and create divergent support surfaces.

### Packaging Requirement

Published skills must work from the files installed by the Skills CLI. Do not require users to keep a separate local source tree beside their target project. After `framework init`, runtime standards and templates must live in the target project under `.open-canal/standards/` and `.open-canal/templates/`; workflow skills must load those paths.

`framework` must carry the runtime copies it needs under `skills/framework/references/standards/` and `skills/framework/references/templates/`, because Skills CLI installation does not expose repository-root files to installed skills.

## Manual Review

After changing skills, inspect:

- skill directory and frontmatter name match;
- required frontmatter fields exist;
- descriptions start with `Use when`;
- referenced runtime standards exist in `.open-canal/standards/` after `framework init`;
- skill bodies stay concise enough to remain routing documents.
