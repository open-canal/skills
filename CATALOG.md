# Skill Catalog

open-canal exposes seven workflow entrypoints that together form a complete software specification methodology chain — from setup (`framework`, `xcode-mcp`) through product planning (`demand`, `version`) and implementation planning (`design`, `develop`, `test`). Users do not need to manually chain every skill; agents trigger the relevant entrypoint on demand. Detailed rules live under `standards/`; skill bodies route agents to the relevant standard and keep only minimal operating guidance. Document skeletons for all generated files live under `templates/`.

## End-to-End Workflow

See `standards/workflow.md` for the full dependency chain, completion criteria, parallel execution rules, and change propagation conventions.

## Setup

| Skill | Use |
| --- | --- |
| `framework` | `init` initializes the specification vault and global project profile; `update` revises product name, product description, or technical approach. |
| `xcode-mcp` | `configure` registers Xcode MCP for the current AI agent; `verify`, `troubleshoot`, and `remove` manage the connection lifecycle. |

## Product Plan

| Skill | Use |
| --- | --- |
| `demand` | `create` creates a new closed-loop requirement PRD into the owning module requirement pool; `update` revises requirement details and propagates changes to downstream docs. |
| `version` | `create` creates a `version/x.y.z.md` iteration file; `add` assigns selected requirements to a version; `remove` removes requirements from a version and resets PRD status. |

## Implementation Plan

| Skill | Use |
| --- | --- |
| `design` | `create` generates a Stitch/Figma AI prototype prompt for a demand PRD and manages the project DESIGN.md design system; `update` revises the design after PRD or DESIGN.md changes. |
| `develop` | `create` generates a client, service, and database technical plan with contracts from a demand PRD; `update` revisits the plan after upstream changes. |
| `test` | `create` generates client, service, and cross-end contract test plans from a develop plan; `update` revises tests after upstream changes. |
