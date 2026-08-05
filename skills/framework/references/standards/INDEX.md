# open-canal Standards Index

This directory is the authoring source for reusable rules and workflows in the open-canal methodology suite. After `framework init`, the runtime copy lives in the target vault at `.open-canal/standards/`, and templates live at `.open-canal/templates/`. `SKILL.md` files should stay concise and route agents to the relevant runtime standard instead of duplicating detailed rules.

## Skill Summary

| Stage | Skill | Standard | Template |
| --- | --- | --- | --- |
| Setup | `framework` | `.open-canal/standards/project-specification.md` | `.open-canal/templates/project.md`, `.open-canal/templates/agents-vault.md`, `.open-canal/templates/claude-vault.md`, `.open-canal/templates/modules-index.md`, `.open-canal/templates/module-index.md`, `.open-canal/templates/version.md` |
| Setup | `xcode-mcp` | `.open-canal/standards/xcode-mcp.md` or skill-local `references/standards/xcode-mcp.md` | N/A |
| Product Plan | `demand` | `.open-canal/standards/demand.md` | `.open-canal/templates/prd.md` |
| Product Plan | `version` | `.open-canal/standards/version.md` | `.open-canal/templates/version.md` |
| Implementation Plan | `design` | `.open-canal/standards/design.md` | `.open-canal/templates/design.md` |
| Implementation Plan | `develop` | `.open-canal/standards/development.md` | `.open-canal/templates/develop.md` |
| Implementation Plan | `test` | `.open-canal/standards/test.md` | `.open-canal/templates/test.md` |

## Standards

- `skill-repository.md`: repository layout, skill packaging, metadata review, and agent discovery.
- `project-specification.md`: Obsidian specification vault initialization and global project context update flow.
- `xcode-mcp.md`: current-agent Xcode MCP setup, verification, troubleshooting, and removal through `xcrun mcpbridge`.
- `repository.md`: module requirement pools, assets, frontmatter, indexes, and migration rules.
- `demand.md`: requirement creation, clarification, PRD placement, and PRD update rules.
- `version.md`: `version/x.y.z.md` iteration creation, requirement assignment, and removal rules.
- `design.md`: Stitch/Figma AI prototype prompt generation, requirement-local design artifacts, and project DESIGN.md design system management.
- `development.md`: client, service, and database technical plan generation for downstream code agents.
- `test.md`: client and service test plan generation from selected develop plans.
- `workflow.md`: end-to-end skill dependency chain, completion criteria, parallel execution rules, and change propagation.
- `standards-management.md`: how to create or update standards in this repository.

## Templates

- `.open-canal/templates/prd.md`: requirement PRD skeleton.
- `.open-canal/templates/design.md`: design artifact skeleton with state matrix and storyboard.
- `.open-canal/templates/develop.md`: development plan skeleton with contracts and task groups.
- `.open-canal/templates/test.md`: test plan skeleton with Given-When-Then tables.
- `.open-canal/templates/version.md`: version file skeleton with readiness checklist.
- `.open-canal/templates/project.md`: global project profile skeleton for product name, product description, and technical approach.
- `.open-canal/templates/agents-vault.md`: AGENTS.md skeleton for the target specification vault.
- `.open-canal/templates/claude-vault.md`: CLAUDE.md skeleton for the target specification vault.
- `.open-canal/templates/module-index.md`: module-level index page skeleton.
- `.open-canal/templates/modules-index.md`: modules top-level index page skeleton.

## Maintenance Rule

When a hard rule changes, update the relevant standard first, then update only the affected skill routing text, template, catalog entry, or example output.
