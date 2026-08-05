# Workflow Overview

## Purpose

This document defines the end-to-end open-canal development workflow: the skill dependency chain, completion criteria per stage, parallelism rules, and change propagation conventions.

## Stage Groups

| Stage | Skills | Scope |
| --- | --- | --- |
| Setup | `framework` | Initialize or update the specification vault and global project context. |
| Setup | `xcode-mcp` | Configure or verify Xcode MCP access for the current AI agent. |
| Product Plan | `demand`, `version` | Define demand PRDs and release/version scope before implementation planning. |
| Implementation Plan | `design`, `develop`, `test` | Translate confirmed demand into prototype prompts, technical plans, and test plans for downstream AI coding agents. |

## Skill Dependency Chain

```
framework init
    ↓
demand ─────────────────┬
    ↓               ↓
design          version create
    ↓               ↓
develop         version add / remove
    ↓               ↓
test ←───────────────┘
```

**Hard dependencies (sequential):**

- `framework init` before any specification workflow skill — the vault must exist before `demand`, `version`, `design`, `develop`, or `test`.
- `xcode-mcp` is independent agent-tooling setup. It may run before or after the open-canal specification workflow and must not create requirement, design, development, test, or version artifacts.
- `demand` before `design`, `develop`, `version add` — requirement-driven work needs a PRD.
- `version create` before `version add` / `version remove` — version add/remove operates on an existing version file.
- `develop` before `test` — test always targets a specific develop.md as its primary source. When develop.md exists but contracts are missing, generate client and service tests; only cross-end contract tests are blocked.

**Soft dependencies (not strictly required, but downstream quality improves when met):**

- `version create` ∥ `demand` — version planning (goals, release type, non-goals) can proceed without PRDs.
- `design` ∥ `version create` — design work and version planning can proceed independently.
- `design` ∥ `develop` — design and technical plan can proceed in parallel after the PRD is confirmed. Only block `develop` on `design` when the PRD is UI-heavy and design decisions materially affect contracts.

**Trigger only (no hard dependency):**

- `version add` / `version remove` — triggered when the user wants to assign or remove requirements from a version. Depends on `version create` having produced a version file and `demand` having produced PRDs.

## Completion Criteria (Definition of Done)

| Stage | Done When |
|-------|-----------|
| `framework init` | Root vault structure exists, `.open-canal/project.md` contains global product context, `.open-canal/AGENTS.md` routes to all skills, root shims redirect, `modules/index.md` exists, `version/0.1.0.md` exists when bootstrapping a new vault, consistency checks pass. |
| `framework update` | `.open-canal/project.md` and derived global summaries reflect the requested product name, product description, or technical approach change without rewriting requirement or version scope. |
| `xcode-mcp configure` | Selected AI agent has an Xcode MCP server entry using `xcrun mcpbridge`; unrelated MCP servers are preserved; registration verification is reported. |
| `xcode-mcp verify` | Selected AI agent lists the server and, when Xcode is open with a project loaded, can reach Xcode tools or reports the exact manual blocker. |
| `xcode-mcp remove` | Selected AI agent no longer lists the Xcode MCP server alias; unrelated MCP servers are preserved. |
| `demand create` | PRD passes the completeness checklist, confirmation gate is passed, module index is updated, version status is set. |
| `demand update` | Change summary produced before writing, affected downstream docs marked, conflicts with design/develop/test/version reported. |
| `version create` | Version file exists with goal, release type, non-goals, risks, and readiness checklist. |
| `version add` | Requirement links appear in the version file, PRD version status updated, readiness gaps reported. |
| `version remove` | Requirement removed from version file, PRD version status set to backlog, readiness checklist updated. |
| `design create` | `design.md` exists with screen list, state matrix, UI element checklist, storyboard order, and prototype prompt. PRD design link updated. |
| `design update` | Changed screens/states updated, stale assets reported, DESIGN.md gaps reported. |
| `develop create` | `develop.md` exists with Client Design, Client Tasks, Service Design, Service Tasks, contracts section when client-service interaction exists, and downstream agent handoff notes. PRD develop link updated. |
| `develop update` | Affected client/service/database sections updated, contracts re-validated, stale tasks reported. |
| `test create` | `test.md` exists with client tests, service tests, cross-end contract tests (or blocked), Given-When-Then format, fixture notes, and evidence locations. PRD test link updated. |
| `test update` | Affected test scenarios updated, removed tests deleted, blocked cases re-assessed. |

## Parallel Execution

Agents may execute parallel-safe stages concurrently when their dependencies are met. Which stages run is determined by the user's request — only run the skills the user asked for.

**Parallel-safe pairs (both can run once common dependency is met):**

| Pair | Common Dependency |
|------|-------------------|
| `demand` ∥ `version create` | `framework init` |
| `design` ∥ `version create` | `demand` |
| `design` ∥ `develop` | `demand` |

**Rule:** Do not run `develop` before `design` if the PRD describes user-facing UI and the design is expected to define contracts.

**Example 1:** User says "create a PRD and design for feature X".
```
framework init → demand create → design create
```

**Example 2:** User says "plan the release version and create the PRD, design, and dev plan for feature X".
```
framework init → (version create ∥ demand create) → design create → develop create
```

## Change Propagation

When a document in the chain changes, downstream docs are marked stale according to these rules:

| Source Changed | Affected Docs | Action |
|----------------|---------------|--------|
| `prd.md` | `design.md`, `develop.md`, `test.md`, `version/x.y.z.md` | Mark linked docs as needing refresh. Update version readiness. |
| `design.md` | `develop.md`, `test.md` | Mark linked docs as needing refresh. Specific to contracts and UI tests. |
| `develop.md` | `test.md` | Mark contract-dependent test cases as needing refresh. |
| `version/x.y.z.md` | requirement `prd.md` | Update PRD version status if requirement was added/removed. |

Marking convention: append `> ⚠️ Stale — source doc changed on YYYY-MM-DD. Review before executing.` to the top of the affected doc's relevant section.

## Agent Handoff

Each skill's output includes explicit handoff notes for the next skill in the chain:

- `demand` reports: PRD path, confirmation status, unresolved product questions, downstream skills to invoke.
- `xcode-mcp` reports: selected agent, command or config path, server name, registration verification, connection verification, and required Xcode manual actions.
- `design` reports: design path, prototype prompt summary, stale assets, PRD gaps blocking design.
- `develop` reports: develop path, task groups, cross-end contracts, blockers for `test`.
- `test` reports: test path, blocked cases, evidence locations, execution order.
