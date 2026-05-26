---
tags:
  - project/routing
---

# Agent Instructions

[Project summary from [[project|Project Profile]].]

This vault is a software project specification knowledge base. It records product plans (demand PRDs and versions) and implementation plans (design, development, and test plans) for AI agents that work in downstream code repositories.

OpenCanal scaffolding lives under `.open-canal/`. Business content lives at root level.

## Skill Routing

Use the following open-canal skills for specification work. Detailed process rules live in `.open-canal/standards/`.

| Stage | When you need to... | Use skill |
|-------|---------------------|-----------|
| Setup | Initialize the specification vault structure | `framework init` |
| Setup | Update global product name, description, or technical approach | `framework update` |
| Product Plan | Create or update a requirement PRD | `demand create` / `demand update` |
| Product Plan | Create a version iteration | `version create` |
| Product Plan | Add requirements to a version | `version add` |
| Product Plan | Remove requirements from a version | `version remove` |
| Implementation Plan | Generate design prototype prompts, or create/update the project DESIGN.md design system | `design create` / `design update` |
| Implementation Plan | Generate technical development plans | `develop create` / `develop update` |
| Implementation Plan | Generate test plans | `test create` / `test update` |

## Repository Structure

```text
.open-canal/             ← open-canal scaffolding
  AGENTS.md             ← this file
  CLAUDE.md
  project.md            ← global product name, description, and technical approach
  design-standards/     ← project visual language, components, principles, interaction
  development-standards/ ← project platform, client, service, database, testing conventions
  standards/            ← open-canal runtime standards
  templates/            ← open-canal runtime templates
modules/                ← business requirements organized by module
  <module-slug>/
    index.md
    requirements/
      <requirement-slug>/
        prd.md          ← required for every accepted requirement
        design.md       ← created by design
        develop.md      ← created by develop
        test.md         ← created by test
        assets/         ← requirement-local assets
version/                ← business version iteration files (x.y.z.md)
  0.1.0.md              ← initial planning placeholder
```

## Conventions

- Use Obsidian wikilinks (`[[path|label]]`) for local document references.
- Frontmatter uses `tags` as a YAML list without leading `#`. PRDs may also include `status` (`draft`, `confirmed`, `in-design`, `in-development`, `in-test`, `done`).
- All other metadata (module, version, priority, dependencies) stays in the document body.
- Every requirement PRD links to its design, develop, and test docs when they exist.
- Version files cross-link to requirement PRDs.
- Global product name, product description, and technical approach live in [[project|Project Profile]].

## Current Modules

- [[../modules/index]]
