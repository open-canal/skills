# Demand Standard

## Demand

Use `demand` to create a new requirement or update an existing requirement detail.

The workflow must guide the user toward one closed-loop requirement point before writing or revising the PRD. A requirement is closed-loop when it has:

- target user or actor;
- triggering scenario;
- user goal;
- start condition;
- main flow;
- success state;
- failure, empty, permission, and cancellation handling when relevant;
- non-goals;
- affected module;
- expected version or backlog state.

## Naming Convention

Requirement slugs use lowercase kebab-case derived from the requirement title, omitting stop words (a, the, of, and, to). Examples:

- "User Login" → `user-login`
- "Add Payment Method for Checkout" → `add-payment-method`

Module slugs follow the same rule: `user-profile`, `order-management`, `admin-dashboard`.

## Confirmation Gate

Before generating or materially rewriting `prd.md`, confirm the requirement boundary with the user unless the user explicitly provided enough detail and asked to write directly.

Confirmation should focus on unresolved product decisions, not implementation details.

## Requirement Placement

The PRD belongs in the owning module requirement pool:

```text
modules/<module-slug>/requirements/<requirement-slug>/prd.md
```

If the module does not exist, create:

```text
modules/<module-slug>/index.md
modules/<module-slug>/requirements/
```

Update `modules/index.md` and the module index when creating or moving a requirement.

## PRD Requirements

Use `.open-canal/templates/prd.md` as the document skeleton. `prd.md` must include:

- requirement summary;
- user or actor;
- scenario and problem;
- goal and non-goals;
- complete user flow with preconditions, main flow, explicit success state, and alternate flows;
- alternate flows must cover: failure, empty state, permission denied, and cancellation (when the feature has user-facing UI; for API-only features, error response handling replaces UI states);
- acceptance criteria (testable, binary pass/fail statements);
- `status` in frontmatter and target version in the body;
- links to design, develop, test, and version docs when they exist.

Keep client, service, database, analytics, and test execution details out of the PRD unless they are user-visible constraints.

## PRD Completeness Checklist

Before confirming a new or materially changed PRD, verify:

- [ ] All closed-loop elements are present.
- [ ] Each alternate flow (failure, empty, permission, cancellation) is covered or explicitly marked N/A with a reason.
- [ ] Acceptance criteria are testable and binary.
- [ ] Non-goals are listed and do not contradict the goal.
- [ ] Module is identified and exists (or is being created).
- [ ] Version status is set (target version or backlog).
- [ ] Links section has entries for design/develop/test (or marked _pending_).

## Updating Existing Requirements

When updating an existing PRD:

1. Read the existing PRD, linked design, develop, test, and version files.
2. Preserve accepted scope unless the user asks to change it.
3. Before writing changes, produce a **change summary** listing: what sections changed, what behavior was added/removed/modified, and why.
4. Update downstream links or mark affected downstream docs as needing refresh.
5. Report any design, development, version, or test file that now conflicts with the PRD.
