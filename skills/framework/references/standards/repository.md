# Repository Documentation Standard

## Routing

Use `.open-canal/AGENTS.md` (or the root `AGENTS.md` shim) first, then read matching index notes recursively. Use Obsidian wikilinks for local notes.

## Module Requirement Pools

Product work is organized by module and requirement pool:

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

Do not insert a business-domain layer under `modules/`. The module is the ownership boundary; the requirement folder is the task boundary.

## Asset Placement

Create assets only under the owning requirement:

```text
modules/<module-slug>/requirements/<requirement-slug>/assets/
```

Never create a repository-level `resources/` or `assets/` dumping ground for requirement deliverables.

Recommended asset folders:

- `prototype/` for Stitch/Figma prompts, exports, screenshots, and prototype outputs;
- `client/` for client-side assets and notes;
- `service/` for service/API artifacts;
- `database/` for schema diagrams or migration notes;
- `test/` for test evidence;
- `references/` for requirement-local supporting material.

## Frontmatter

Every generated project Markdown file should use Obsidian official frontmatter only. For this repository's generated docs:

- Every document must have `tags`.
- `prd.md` may also include `status`.
- All other metadata belongs in the document body or index tables.

Rules:

- `tags` uses Obsidian's official tags property, formatted as a YAML list without leading `#`.
- Use lowercase kebab-case tags and `/` for nested tags, for example `project/requirement` and `doc/prd`.
- Put module, requirement name, version, priority, dependencies, source-of-truth notes, and update dates in the document body or index tables, not in frontmatter. PRD `status` is the exception: it belongs in PRD frontmatter.

Example:

```yaml
---
tags:
  - project/requirement
  - doc/prd
---
```

## Index Synchronization

Creating or moving a module or requirement requires updating:

- `modules/index.md`;
- the owning `modules/<module-slug>/index.md`;
- every affected `version/x.y.z.md`;
- backlinks inside requirement docs.

Legacy feature folders, global resources, and package-style version folders should be migrated or marked as manual migration items before new work depends on them.

## Consistency Review

Check each requirement folder as a unit: PRD, design, development plan, test plan, assets, module index, and version links.

High-risk checks:

- one requirement trying to cover multiple closed loops;
- product behavior defined only in design/develop/test docs;
- version membership not reflected in PRD;
- design prompt stale after PRD change;
- client/service/database task mismatch;
- missing assets folder for generated design or test outputs;
- future work leaking into the selected version.

## Migration

When migrating legacy docs:

1. Inventory old docs, active links, and source-of-truth claims.
2. Map each active fact to a module, requirement PRD, design artifact, development plan, test plan, version file, or decision record.
3. Move or rewrite only the minimum required content.
4. Update indexes and backlinks.
5. Run a link/frontmatter consistency pass.

Do not keep two active facts for the same behavior.
