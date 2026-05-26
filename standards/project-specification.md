# Project Specification Standard

## Purpose

Initialize and maintain an Obsidian-compatible software project specification vault. The vault records requirements, designs, development plans, versions, and test plans for AI agents that work in downstream code repositories.

Use `framework` with a command parameter:

- `framework init` initializes an empty or missing specification vault;
- `framework update` updates global project description and framework-level vault context without destroying existing user-authored content.

## Target Vault Root

The **target vault root** is the agent's current working directory — the user's project repository. All scaffold output (`.open-canal/`, `.obsidian/`, `modules/`, `version/`) goes to the target root. Never create specification vault scaffolding inside the open-canal skill repository, a global skill directory, or an agent configuration directory.

Runtime standards and templates are installed into the target vault at `.open-canal/standards/` and `.open-canal/templates/`. Generated documents are written to `{target-root}/`.

## Command Parameters

`framework` accepts two command parameters:

- `init` — create or repair the target vault scaffold.
- `update` — update global project context after the product name, product description, or technical approach changes.

If the user does not provide a command, infer `init` when `.open-canal/` is missing and infer `update` when `.open-canal/` exists and the request is about global project context. Ask only when the command cannot be inferred.

## Project Profile

Project-level identity lives in `.open-canal/project.md`. This is the source of truth for global context that applies across requirements, versions, designs, development plans, and tests.

During `framework init`, ask exactly these required questions before writing the scaffold:

1. Product name: what is the product called?
2. Product description: what does the product do?
3. Technical approach: what broad platform or architecture is planned (web app, iOS app, desktop app, API service, etc.)? Include concrete frameworks when known.

Write the answers to `.open-canal/project.md`. Also sync the derived one-sentence project summary into `.open-canal/AGENTS.md`, and sync the technical approach into `.open-canal/development-standards/platform.md`.

During `framework update`, update `.open-canal/project.md` and the derived global summaries only. Do not rewrite requirement PRDs, design prompts, development plans, test plans, or version files unless the user explicitly asks.

## Initialization Output

Create documentation infrastructure only. Do not generate application code, project-local child skills, or executable helpers.

Minimum root structure:

```text
.open-canal/
  AGENTS.md
  CLAUDE.md
  project.md
  design-standards/
    visual.md
    components.md
    principles.md
    interaction.md
  development-standards/
    platform.md
    client.md
    service.md
    database.md
    testing.md
  standards/
    INDEX.md
    project-specification.md
    repository.md
    demand.md
    version.md
    design.md
    development.md
    test.md
    workflow.md
    skill-repository.md
    standards-management.md
  templates/
AGENTS.md           ← shim: redirects to .open-canal/AGENTS.md
CLAUDE.md           ← shim: redirects to .open-canal/AGENTS.md
.obsidian/
  app.json           ← minimal config (see below)
  graph.json          ← minimal config (see below)
modules/
  index.md
version/
  0.1.0.md
```

### Root Shim Files

Root-level `AGENTS.md` and `CLAUDE.md` are minimal compatibility shims that redirect agents to `.open-canal/`. Do not put routing rules in these files.

`AGENTS.md` at root:

```markdown
# Agent Instructions

See `.open-canal/AGENTS.md` for the full specification vault routing guide and conventions.

OpenCanal scaffolding lives under `.open-canal/`. Business content (requirements, versions) lives at root level:

- `modules/` — business module requirement pools
- `version/` — version iteration files
```

`CLAUDE.md` at root:

```markdown
# Claude Code Entrypoint

See `.open-canal/AGENTS.md` for the full specification vault routing guide and conventions.
```

### .open-canal/AGENTS.md

Use `.open-canal/templates/agents-vault.md` as the skeleton. `.open-canal/AGENTS.md` is the canonical routing guide. It must include:

- project summary derived from `.open-canal/project.md`;
- vault purpose statement;
- skill routing table grouped by setup, Product Plan, and Implementation Plan;
- directory structure overview showing `.open-canal/`, `modules/`, and `version/`;
- conventions: Obsidian wikilinks, frontmatter format, cross-linking policy;
- link to `[[project|Project Profile]]`;
- link to `[[../modules/index|Module Index]]`.

### .open-canal/project.md

Use `.open-canal/templates/project.md` as the skeleton. The file must contain only global project context:

- product name;
- product description;
- technical approach;
- last updated date.

Do not store version goals, requirement scope, release notes, or feature-specific decisions here.

### .open-canal/CLAUDE.md

`.open-canal/CLAUDE.md` is a minimal entrypoint file. Its content:

```markdown
---
tags:
  - project/routing
---

# Claude Code Entrypoint

Read `AGENTS.md` for the full specification vault routing guide and conventions.
```

### .open-canal/design-standards/

Initialize with a skeleton file in each of the four files so the vault is immediately usable. Each file starts with a YAML frontmatter and a single comment directing the project team to fill in details.

> **Note:** `DESIGN.md` is NOT created by `framework init`. It is created by `design` on first use after the project team fills in the four skeleton files or provides design references.

#### `visual.md`

```markdown
---
tags:
  - design-standard
  - visual
---

# Visual Language

> Replace this comment with your project's visual language:
> - Color palette (primary, accent, warning, destructive, neutral — with hex values)
> - Typography (font families, scale, weights)
> - Spacing scale
> - Border radius tokens
> - Shadow tokens
```

#### `components.md`

```markdown
---
tags:
  - design-standard
  - components
---

# Component Library

> Replace this comment with your project's component catalog:
> - Primitives (Button, Input, Card, etc.)
> - Composite components
> - States per component (default, hover, active, disabled, loading, error)
> - Size variants
```

#### `principles.md`

```markdown
---
tags:
  - design-standard
  - principles
---

# Design Principles

> Replace this comment with your project's design principles.
> Examples: "Clarity over density", "Mobile-first", "Progressive disclosure".
```

#### `interaction.md`

```markdown
---
tags:
  - design-standard
  - interaction
---

# Interaction Patterns

> Replace this comment with your project's interaction conventions:
> - Navigation patterns
> - Gesture conventions
> - Animation and motion rules
> - Accessibility interaction requirements
```

### .open-canal/development-standards/

Initialize with a skeleton file in each of the five files so the vault is immediately usable. Each file starts with a YAML frontmatter and a single comment directing the project team to fill in details. `develop` and `test` MUST read these files before generating plans.

#### `platform.md`

```markdown
---
tags:
  - development-standard
  - platform
---

# Platform & Tech Stack

Technical approach from `.open-canal/project.md`: [web app, iOS app, desktop app, API service, etc.; include concrete frameworks when known.]

> Replace this comment with your project's platform conventions:
> - Runtime (Node 20, Swift 5.9, etc.)
> - Framework (Next.js App Router, SwiftUI, etc.)
> - Package manager and workspace layout
> - Build tooling and bundler
> - Monorepo structure if applicable
> - CI/CD pipeline and deployment targets
```

#### `client.md`

```markdown
---
tags:
  - development-standard
  - client
---

# Client Development Conventions

> Replace this comment with your project's client-side conventions:
> - Component architecture and file structure
> - State management pattern
> - Routing and navigation conventions
> - API client setup (base URL, interceptors, error handling)
> - Offline and caching strategy
> - Permissions and feature flags on the client
> - Accessibility requirements beyond design standards
```

#### `service.md`

```markdown
---
tags:
  - development-standard
  - service
---

# Service Development Conventions

> Replace this comment with your project's service-side conventions:
> - API design conventions (REST, GraphQL, RPC)
> - Auth model (JWT, session, OAuth) and token flow
> - Authorization pattern (RBAC, ABAC, custom)
> - Error response format and status code conventions
> - Logging, metrics, and observability
> - Rate limiting and throttling defaults
```

#### `database.md`

```markdown
---
tags:
  - development-standard
  - database
---

# Database Conventions

> Replace this comment with your project's data conventions:
> - Database engine and version
> - Naming conventions (tables, columns, indexes)
> - Migration tool and strategy
> - Idempotency pattern (keys, deduplication)
> - Soft delete vs hard delete policy
> - Backup and retention policy
```

#### `testing.md`

```markdown
---
tags:
  - development-standard
  - testing
---

# Testing Conventions

> Replace this comment with your project's testing conventions:
> - Test runner and assertion library
> - Client test framework (Playwright, Testing Library, XCUITest)
> - Service test framework (Supertest, XCTest, Jest)
> - Fixture and seed data management
> - Mock vs real services policy
> - CI test execution environment
```

### .obsidian/

Minimal Obsidian configuration files. `app.json` enables wikilinks and sets the vault name to the repository name:

```json
{
  "newLinkFormat": "shortest",
  "useMarkdownLinks": false,
  "showUnsupportedFiles": true
}
```

`graph.json` enables the local graph view with default filters:

```json
{
  "collapse-filter": true,
  "search": "",
  "showTags": false,
  "showAttachments": false,
  "hideUnresolved": false,
  "showOrphans": true,
  "collapse-color-groups": true,
  "colorGroups": [],
  "collapse-display": true,
  "showArrow": false,
  "textFadeMultiplier": 0,
  "nodeSizeMultiplier": 1,
  "lineSizeMultiplier": 1,
  "collapse-forces": true,
  "centerStrength": 0.518713248970312,
  "repelStrength": 10,
  "linkStrength": 1,
  "linkDistance": 250,
  "scale": 1,
  "close": true
}
```

## Version Bootstrap

During `framework init`, create `version/0.1.0.md` if no `version/x.y.z.md` files exist.

Use `.open-canal/templates/version.md` as the skeleton and change only the title to `# Version 0.1.0`. Leave version-specific planning fields as placeholders until `version` is explicitly requested. Do not copy product name, product description, technical approach, or any initialization summary into the `0.1.0` version goal, requirement list, release notes, or decision log.

Do not ask version-specific questions during `framework init`.

## Updating Non-Empty Vaults

When the target repository is not empty:

1. Inspect existing root files, `.open-canal/`, `.open-canal/project.md`, indexes, modules, feature folders, requirements, versions, templates, and design standards.
2. Preserve user-authored requirement, design, technical, and test content.
3. Add missing root structure before moving content.
4. If `.open-canal/` does not exist, create it and populate with current open-canal scaffolding.
5. Migrate old feature-level content into module requirement pools only when ownership is clear.
6. Leave uncertain content in place and report it as a manual migration item.
7. Update indexes and backlinks after every move.

Do not treat initialization as a destructive reset.

For `framework update`, treat `.open-canal/project.md` as the main editable artifact. Update `.open-canal/AGENTS.md` and `.open-canal/development-standards/platform.md` only where they mirror project profile fields. Preserve all requirement-level and version-level documents.

## Requirement Pool Model

Requirements live under the owning module at root level:

```text
modules/<module-slug>/
├── index.md
└── requirements/
    └── <requirement-slug>/
        ├── prd.md
        ├── design.md
        ├── develop.md
        ├── test.md
        └── assets/
            ├── prototype/
            ├── client/
            ├── service/
            ├── database/
            ├── test/
            └── references/
```

`prd.md` is required for every accepted requirement. `design.md`, `develop.md`, and `test.md` are created when the corresponding workflow runs.

## Template Rules

- Every generated Markdown document uses the corresponding template from `.open-canal/templates/` as its skeleton. Workflow skills must not require a separate local source tree after installation.
- All generated documents must include YAML frontmatter with `tags`. Exceptions: root-level AGENTS.md / CLAUDE.md shims and `.open-canal/CLAUDE.md` (routing-only files without `tags` are acceptable).
- `prd.md` frontmatter may include `tags` and `status`. No other frontmatter fields are allowed.
- `status` values: `draft`, `confirmed`, `in-design`, `in-development`, `in-test`, `done`.
- All other metadata (module, name, version, priority, dependencies, source-of-truth notes, update dates) belongs in the document body or index tables, not in frontmatter.
- `tags` must be a YAML list without leading `#`; use `/` for nested tags.
- Local documents use Obsidian wikilinks for traceability.
- All generated documents must follow the [Obsidian Style Guide](https://docs.obsidian.md/help/style-guide#Terminology%20and%20grammar) for terminology, grammar, formatting, and Markdown conventions.
- Design templates and the design workflow reference `DESIGN.md` as the primary design system source of truth. The four skeleton files under `.open-canal/design-standards/` are raw material for DESIGN.md synthesis; they are not directly linked from templates.
- Development and test templates reference `.open-canal/development-standards/` as the primary technical convention source. `develop` and `test` must read these files before generating plans; when a repeated technical decision is discovered during plan generation, suggest promoting it into the relevant development-standards file.
- `.open-canal/project.md` is the global project profile. Keep product name, product description, and technical approach there rather than duplicating them into requirement or version documents.
- Generated application code belongs in downstream code repositories, not in the specification vault.

## Consistency Checks

Agents should verify:

- root-level `AGENTS.md` and `CLAUDE.md` shims exist and redirect to `.open-canal/`;
- `.open-canal/AGENTS.md` exists and routes to all skills;
- `.open-canal/project.md` exists and contains product name, product description, and technical approach;
- `.open-canal/standards/` exists with all open-canal runtime standards;
- `.open-canal/design-standards/` contains the four skeleton files;
- `.open-canal/development-standards/` contains the five skeleton files;
- `.open-canal/templates/` exists with all open-canal runtime templates;
- `modules/index.md` exists and links to module indexes;
- `version/0.1.0.md` exists when no other version file existed before initialization;
- each active module has `requirements/`;
- each accepted requirement has `prd.md`;
- generated design/development/test files sit inside the owning requirement folder;
- every generated requirement asset is under the requirement-local `assets/` folder;
- version files use `version/x.y.z.md`;
- links between requirement PRDs, design prompts, development plans, tests, and version files are bidirectional where applicable.
