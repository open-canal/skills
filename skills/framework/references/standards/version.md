# Version Standard

## Version Create

Use `version` with `create` to create a version iteration file.

Version files live directly under `version/` and use exact semver file names:

```text
version/x.y.z.md
```

Do not create `versions/`, `v1`, `v1.0.0`, or version package directories unless the target vault has an explicit local override.

Exception: `framework init` creates `version/0.1.0.md` as an initial planning placeholder when no version files exist. That bootstrap file must not infer a version goal from project initialization context; version-specific planning still belongs to `version create`, `version add`, or `version remove`.

### Semver Derivation

When the user does not specify a version number:

1. Scan existing `version/x.y.z.md` files.
2. If the user describes the change as a bug fix or patch → bump patch (z+1).
3. If the user describes a new feature → bump minor (y+1, z=0).
4. If the user describes breaking changes → bump major (x+1, y=0, z=0).
5. If the user's intent is ambiguous (e.g. "just a small change"), default to patch (z+1).
6. If no prior versions exist, start at `0.1.0`.
7. Always confirm the derived version with the user before creating the file.

### Version File Content

Use `.open-canal/templates/version.md` as the document skeleton. Each version file must include:

- **version goal** (required) — one sentence of user-facing outcome;
- **release type** (required) — one of `major`, `minor`, `patch`, `hotfix`;
- **requirement list** (required when `version add` has run) — table linking to PRDs with readiness columns;
- **non-goals** (required) — explicitly excluded work;
- **key risks** (required) — at minimum, one risk identified or "none identified";
- **readiness checklist** (required);
- **decision log** (optional, populate as decisions are made);
- **release notes draft** (optional, populate before release).

### Release Type

| Type | When |
|------|------|
| `major` | Breaking changes, new architecture, significant UX overhaul. |
| `minor` | New features, backward-compatible. |
| `patch` | Bug fixes, performance improvements, no new features. |
| `hotfix` | Urgent production fix, created off latest release tag. |

Every requirement entry must link to its PRD:

```text
[[modules/<module-slug>/requirements/<requirement-slug>/prd]]
```

## Version Add

Use `version` with `add` to select one or more requirements and add them to an existing version.

Before adding:

1. Read the requirement PRD.
2. Check whether design, develop, and test docs exist or are still pending.
3. Confirm the requirement is small enough for the target iteration.
4. Check for duplicate or conflicting version membership.

After adding:

1. Update `version/x.y.z.md` requirement list and readiness checklist.
2. Update the requirement PRD's version section.
3. Update the module index if it tracks version state.
4. Report missing design, development, or test documents as readiness gaps.

## Version Remove

Use `version` with `remove` to remove one or more requirements from an existing version.

1. Read the version file and affected requirement PRDs.
2. Remove the requirement row from the version requirement list.
3. Update the requirement PRD's version status to `backlog`.
4. Update the version readiness checklist.
5. Preserve design, develop, and test docs — they remain valid for future version assignments.

## Scope Rules

- A version contains requirements, not loose feature ideas.
- Future work must stay outside the requirement list unless explicitly selected.
- A requirement is too large for a version when it: spans more than one module, contains more than 5 distinct user flows, or lacks a clear closed-loop boundary. Split it through `demand` before adding it to a version.
- Removing a requirement from a version requires updating the version file and requirement PRD together.
- All requirements in a version contribute to the same release goal; if a requirement does not align, it belongs in a different version.
