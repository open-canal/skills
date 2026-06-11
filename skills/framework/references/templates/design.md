---
tags:
  - project/design
  - doc/design
---

# Design: [Requirement Title]

## Source

PRD: [[./prd]]
DESIGN.md: [[.open-canal/design-standards/DESIGN.md]] _(loaded and validated)_
Target tool: [Stitch | Figma]
Target platform: [iOS | Android | Web | Desktop]
Viewport: [e.g. 390x844 for iPhone 14]

## DESIGN.md Coverage

### Token Layer

> YAML tokens are the normative values. If prose and tokens conflict, resolve the conflict before generating prototype frames.

| Token Group | Values Used in This Requirement |
|-------------|----------------------------------|
| `colors` | [Primary (#...), surface (#...), text (#...), status colors (#...)] |
| `typography` | [h1, body-md, label-md, etc.] |
| `rounded` | [sm/md/lg/full values used] |
| `spacing` | [base scale, gutters, margins, section spacing] |
| `components` | [button-primary, input, card, nav, domain-specific components] |

### Markdown Sections

| Section | Guidance Applied |
|---------|------------------|
| `Overview` | [Brand personality, density, emotional tone.] |
| `Colors` | [Palette roles and contrast guidance.] |
| `Typography` | [Hierarchy, family, weight, line-height, label rules.] |
| `Layout` | [Grid, breakpoints, spacing, containment.] |
| `Elevation & Depth` | [Shadow, border, tonal-layer strategy.] |
| `Shapes` | [Corner radius and shape language.] |
| `Components` | [Component styling and variants.] |
| `Do's and Don'ts` | [Generation guardrails and pitfalls.] |

## Screens

| # | Screen Name | Description |
|---|-------------|-------------|
| 1 | [Screen A] | [What the user sees/does.] |
| 2 | [Screen B] | |

## State Matrix

| Screen | Normal | Loading | Empty | Error | Permission Denied |
|--------|--------|---------|-------|-------|--------------------|
| [A] | [desc] | [desc] | [desc] | [desc] | [desc] |
| [B] | [desc] | [desc] | [desc] | [desc] | [desc] |

## UI Element Checklist per Screen

### Screen [A]

- [ ] [Element 1 - e.g. title bar, button, input]
- [ ] [Element 2]
- [ ] [Accessibility: focus order, labels, contrast]

### Screen [B]

- [ ] [Element 1]

## Storyboard Order

1. Entry: [Screen X, Normal]
2. Pre-action: [Screen Y, Normal]
3. Feedback / Loading: [Screen Y, Loading]
4. Result: [Screen Z, Normal]
5. Cancel / Back: [Screen W]
6. Failure Recovery: [Screen Y, Error]

## Interactions

- [Interaction 1: trigger -> response]
- [Interaction 2: trigger -> response]

## Prototype Prompt Constraints

> All visual style MUST come from DESIGN.md. Quote specific token values and section guidance.

- **Colors:** [Use DESIGN.md `colors` tokens and role descriptions.]
- **Typography:** [Use DESIGN.md `typography` tokens and hierarchy prose.]
- **Layout:** [Use DESIGN.md `spacing` tokens and `Layout` guidance.]
- **Elevation:** [Use `Elevation & Depth` guidance.]
- **Shapes:** [Use `rounded` tokens and `Shapes` guidance.]
- **Components:** [Use `components` tokens and `Components` prose, including variants.]
- **Accessibility:** [Contrast and touch target rules from DESIGN.md; if absent, target platform minimums.]
- **Guardrails:** [Relevant `Do's and Don'ts`.]

## Stitch Sync

> Fill this section when the target tool is Stitch.

- DESIGN.md import method: [not used | pasted | uploaded | imported from URL/GitHub | synced from local asset]
- Stitch project design system: [not created | created | updated | selected existing]
- Project default: [not set | set for future screens]
- Existing screens needing manual re-application: [none | list screens]
- Export expectation: [DESIGN.md included with exported project zip | N/A]

## Assets

- Prototype: `assets/prototype/`
- References: `assets/references/`

## Stale Assets

> Update after design changes: list removed or outdated prototype files here.

- _(none)_

## Acceptance Notes

- [ ] [Note 1 for prototype review - e.g. "Screen X loading state matches PRD empty flow"]
- [ ] [Note 2]
