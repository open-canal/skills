# Standards Management Standard

## Create A Standard

Create a new standard when a workflow or rule set applies across multiple skills or should remain stable outside one skill body.

Before creating:

1. Search `standards/INDEX.md` and existing standard files.
2. Confirm the new rule cannot live cleanly as a section in an existing standard.
3. Choose a concise lowercase kebab-case file name.
4. Add the file under `standards/`.
5. Add it to `standards/INDEX.md`.
6. Update every affected `SKILL.md` to route to the new standard.
7. Update `CATALOG.md` when skill responsibilities change.
8. Manually review the changed standard, affected skills, and catalog for drift.

## Update A Standard

Use the smallest coherent edit set.

1. Identify the owning standard.
2. Update rules in the standard first.
3. Update affected skills only if trigger conditions, routing, or outputs changed.
4. Update templates, references, catalog entries, and example outputs that encode the old rule.
5. Manually review affected files against the new rule.

## Separation Rule

Keep these responsibilities separate:

- `standards/`: durable rules and process details.
- `skills/*/SKILL.md`: trigger conditions, which standard to read, and minimal operating guidance.
- `templates/`: generated document skeletons.
- `references/`: large optional background material.

Do not duplicate detailed rules across standards and skill bodies.

## Review Checklist

- Does the rule have one source of truth?
- Do affected skills point to that source?
- Did any template, reference, catalog entry, or example output still encode the old rule?
- Do changed skills still follow the repository review rules in `standards/skill-repository.md`?
