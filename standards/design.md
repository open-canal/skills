# Design Standard

## DESIGN.md - Project Design System

A project-level `DESIGN.md` is required as the semantic design system source of truth. It is the design counterpart to `AGENTS.md`: agents read it to keep generated screens visually consistent across the project.

The format must stay compatible with Stitch DESIGN.md. When the target prototype tool is Figma or another tool, `DESIGN.md` still serves as the design reference, but Stitch upload/default-screen behavior is optional.

Reference documents:

- [Stitch DESIGN.md overview](https://stitch.withgoogle.com/docs/design-md/overview/)
- [Import from your codebase](https://stitch.withgoogle.com/docs/design-md/get-instructions/)
- [The DESIGN.md specification](https://stitch.withgoogle.com/docs/design-md/specification/)
- [View, edit, and export](https://stitch.withgoogle.com/docs/design-md/usage/)

### Location

OpenCanal stores the project design system at:

```text
.open-canal/design-standards/DESIGN.md
```

When working directly with Stitch, paste, upload, import, or otherwise register the same DESIGN.md content as the Stitch project design system. Keep any local Stitch asset copy synced with `.open-canal/design-standards/DESIGN.md`.

### File Format

`DESIGN.md` has two layers:

1. **YAML front matter tokens** - machine-readable values that agents and tools use as normative design data.
2. **Markdown body sections** - human-readable design rationale that explains how and why to apply the tokens.

The YAML front matter must start and end with a line containing exactly `---`. Do not create a tags-only front matter block. Use real design tokens, and omit token groups only when the project truly does not define them yet.

The file content must be self-contained. Do not leave unresolved references to local source files, CSS variables, Figma-only names, screenshots, or repository paths; translate them into concrete tokens and prose guidance.

```yaml
---
version: alpha
name: [Design System Name]
description: [Optional short description]
colors:
  primary: "#1A1C1E"
  secondary: "#6C7278"
  tertiary: "#B8422E"
  neutral: "#F7F5F2"
typography:
  h1:
    fontFamily: Public Sans
    fontSize: 48px
    fontWeight: 600
    lineHeight: 1.1
    letterSpacing: -0.02em
  body-md:
    fontFamily: Public Sans
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.6
rounded:
  sm: 4px
  md: 8px
  full: 9999px
spacing:
  sm: 8px
  md: 16px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.neutral}"
    rounded: "{rounded.md}"
    padding: 12px
---
```

### Token Schema

| Token group | Value |
|---|---|
| `version` | Optional string. Current Stitch spec version is `alpha`. |
| `name` | Required design system name. |
| `description` | Optional short description. |
| `colors` | Map of token name to any valid CSS color string. Prefer `#RRGGBB` hex values for broad tooling support. At least a primary palette should be defined. |
| `typography` | Map of token name to typography object. |
| `rounded` | Map of scale level to dimension, for example `4px`, `8px`, `9999px`. |
| `spacing` | Map of scale level to dimension or unitless number. |
| `components` | Map of component name to property tokens. Values may be literals or token references. |

Supported token value types:

| Type | Format | Example |
|---|---|---|
| Color | Any valid CSS color string; prefer `#RRGGBB` hex values | `"#1A1C1E"`, `"oklch(62% 0.18 250)"` |
| Dimension | number plus `px`, `em`, or `rem` | `48px`, `-0.02em` |
| Token reference | `{path.to.token}` | `{colors.primary}` |
| Typography | composite object | `fontFamily`, `fontSize`, `fontWeight`, `lineHeight`, `letterSpacing` |

Typography tokens may include `fontFamily`, `fontSize`, `fontWeight`, `lineHeight`, `letterSpacing`, `fontFeature`, and `fontVariation`. Prefer unitless `lineHeight` where possible.

Component token properties commonly include `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`, `size`, `height`, and `width`. Variants such as hover or active states should be represented as related component entries, for example `button-primary-hover`.

Unknown token names and custom token groups should be preserved when they contain valid values. Duplicate canonical section headings are errors and must be fixed before use.

### Canonical Sections

The markdown body uses `##` headings. Sections may be omitted when they are not relevant, but sections that are present should appear in this order:

| # | Section | Alias | Content |
|---|---|---|---|
| 1 | `Overview` | `Brand & Style` | Brand personality, audience, emotional response, density, and general look and feel. |
| 2 | `Colors` | | Palette roles and rationale. Names in prose should correspond to token values in YAML. |
| 3 | `Typography` | | Type hierarchy, families, weights, sizes, spacing, and semantic roles. |
| 4 | `Layout` | `Layout & Spacing` | Grid models, spacing scales, containment, breakpoints, and responsive principles. |
| 5 | `Elevation & Depth` | `Elevation` | Shadow strategy or alternatives such as borders, tonal layers, and color contrast. |
| 6 | `Shapes` | | Corner radii, edge treatments, and overall shape language. |
| 7 | `Components` | | Buttons, chips, lists, inputs, checkboxes, radio buttons, tooltips, cards, navigation, and domain-specific components. |
| 8 | `Do's and Don'ts` | | Practical generation guardrails and pitfalls. |

An optional top-level `#` heading may be used as the document title. It is not parsed as a design section.

### Language Rules

- Treat YAML tokens as normative. When prose and tokens conflict, update one or both before generating screens.
- Use exact token values for colors, typography, spacing, roundedness, and component properties.
- Use prose to explain intent, hierarchy, and application rules.
- Prefer descriptive color names in prose with exact hex values when useful, for example `Primary (#1A1C1E)`.
- Explain the functional role of every visual decision, not only its appearance.
- Preserve unknown sections and custom tokens unless they duplicate canonical section headings or contradict active tokens.

## Design

Use `design` with `create` to generate a Stitch/Figma AI prototype prompt for a selected demand PRD. Use `design` with `update` to update an existing design after PRD or DESIGN.md changes.

The design artifact belongs in the same requirement pool as the PRD:

```text
modules/<module-slug>/requirements/<requirement-slug>/design.md
```

The workflow must also create the requirement-local assets folder when missing:

```text
modules/<module-slug>/requirements/<requirement-slug>/assets/
```

Prototype outputs and exports should stay under `assets/prototype/` or `assets/references/` unless the target vault defines a stricter local split.

## Inputs

Read (paths below are relative to the target specification vault root):

- the selected requirement `prd.md`;
- **project `.open-canal/design-standards/DESIGN.md`** - required; when missing, fail early by switching to the DESIGN.md creation workflow and requesting user approval before any prototype prompt generation;
- existing `design.md` when updating;
- project `.open-canal/design-standards/` for additional style constraints;
- linked version file when version scope changes the screen set.

### DESIGN.md Prerequisite Gate

Before generating any prototype prompt, verify that `.open-canal/design-standards/DESIGN.md` exists and is compatible with [The DESIGN.md specification](https://stitch.withgoogle.com/docs/design-md/specification/).

| State | Action |
|-------|--------|
| **Missing** | Switch to DESIGN.md creation workflow. Synthesize from available inputs. Request user approval. Do not proceed with prototype prompts until DESIGN.md is approved. |
| **Invalid** | Report concrete validation problems: malformed front matter, missing design system name, invalid token values, duplicate canonical sections, or token/prose conflicts. Do not proceed until fixed. |
| **Underspecified** | Report missing design coverage that blocks the target screens, such as no primary color, no readable typography, no layout guidance, or missing component guidance for required UI controls. Offer to update DESIGN.md before prototype prompts. |
| **Usable** | Proceed to prototype prompt generation. Quote relevant tokens and markdown sections directly. |

### PRD Ambiguity Gate

Before generating a prompt, check the PRD for ambiguity. The PRD is ambiguous and must be clarified when:

- any closed-loop element (actor, scenario, goal, flow, success, failure, empty, permission, cancellation) is missing and not marked N/A;
- the main flow skips from trigger to result without describing intermediate user actions;
- screen boundaries are unclear (two distinct pages described as one, or one page split across two);
- acceptance criteria use non-observable language (e.g. "the system should be fast" instead of "loads within 2 seconds").

Report specific gaps to the user. Do not fill in missing product behavior with design assumptions.

## Design Document Requirements

Use `.open-canal/templates/design.md` as the document skeleton. `design.md` must include:

- source PRD link;
- target prototype tool, such as Stitch or Figma;
- **DESIGN.md reference** - link to `.open-canal/design-standards/DESIGN.md` and confirm it was loaded;
- **DESIGN.md token summary** - list the relevant `colors`, `typography`, `rounded`, `spacing`, and `components` tokens used for this requirement;
- **DESIGN.md section summary** - cite the relevant canonical sections (`Overview`, `Colors`, `Typography`, `Layout`, `Elevation & Depth`, `Shapes`, `Components`, `Do's and Don'ts`);
- **screen list** - every screen the user will see, one row per screen;
- **state matrix** - a table of screen x state (Normal, Loading, Empty, Error, Permission Denied) with a short description per cell;
- **UI element checklist** - per screen, list every interactive and display element;
- **storyboard order** - numbered sequence of frames in user-action order;
- **interaction details** - trigger -> response pairs per interaction;
- Stitch design-system sync notes when Stitch is the target tool;
- asset output locations;
- acceptance notes for prototype review.

Do not write client architecture, API, WebSocket, database, cache, or migration details in `design.md`.

## Prototype Prompt Requirements

Every generated prompt must include:

- product and requirement context;
- exact screens to generate;
- states per screen;
- UI element checklist per screen;
- flow storyboard order;
- key interactions;
- **visual style constraints from DESIGN.md** - quote relevant token values and markdown rationale directly;
- component style guidance from DESIGN.md `components` tokens and `## Components` prose;
- target platform and viewport;
- resource/output location expectations.

For interactive requirements, output frames in user-action order: entry, pre-action, feedback/loading/confirmation/permission, result, cancel/back, and failure recovery.

## Visual Rules

- One frame shows exactly one page and one state.
- Colors use DESIGN.md color tokens and their documented roles. Do not invent colors when a token exists.
- Typography follows DESIGN.md typography tokens and prose hierarchy.
- Layout and spacing follow DESIGN.md `spacing` tokens and `## Layout` guidance.
- Component styling follows DESIGN.md `components` tokens and `## Components` prose. Document deviations explicitly when requirement-specific needs override.
- Elevation and shape choices follow `## Elevation & Depth`, `## Shapes`, and the `rounded` token scale.
- Touch targets and contrast must follow DESIGN.md guardrails; when not specified, meet the target platform accessibility minimum.
- When DESIGN.md is missing, invalid, or too underspecified for the requested UI, alert the user and pause. Do not substitute generic design tokens.
- Avoid unrelated platform visual language, low-contrast text, inconsistent spacing, and custom gestures without fallback.

## Update Rules

When updating design after PRD changes:

1. Compare old and new PRD behavior. List every user-visible change before editing the design.
2. Check DESIGN.md for staleness - if new PRD behavior requires tokens, components, or guardrails not in DESIGN.md, report the gap.
3. Update only affected screens, states, and prompt sections.
4. Keep previous prototype asset links when still valid. An asset is still valid when the screen layout, state, and key interactions it represents have not changed.
5. Mark stale assets in the `design.md` Stale Assets section. A stale asset is any prototype file that no longer matches the updated PRD behavior. Include the file path and reason.
6. If Stitch is the target tool and a design system is uploaded or selected as project default, remember that future screens inherit it automatically but existing screens are not retroactively updated. List any existing screens that must be manually reapplied.
7. Report the stale asset list and Stitch sync notes alongside the updated design artifact.

## DESIGN.md Maintenance

### When to create or update DESIGN.md

- **Create** when `.open-canal/design-standards/DESIGN.md` does not exist and the project has at least one screen to generate.
- **Update** when:
  - A new platform or viewport is targeted.
  - PRD changes introduce new component patterns not documented in DESIGN.md.
  - Visual style constraints change (rebrand, design tokens update).
  - A design review reveals inconsistent generated output that should be codified.
  - Stitch edits change structured tokens or the DESIGN.md summary.

The `design` workflow must always target a specific demand PRD, and on create/update it must update both the `design.md` artifact and the source PRD's design link.

### DESIGN.md creation workflow

1. Determine whether the source of truth is an existing Stitch project, an existing codebase, a brand URL/image, user-provided design direction, or manual authoring.
2. Draft YAML token front matter using the official schema. Include `name`, at least the primary color palette when known, typography tokens when known, and any known `rounded`, `spacing`, and `components` tokens.
3. Draft markdown body sections in canonical order: `Overview`, `Colors`, `Typography`, `Layout`, `Elevation & Depth`, `Shapes`, `Components`, `Do's and Don'ts`. Omit only sections that are not relevant or cannot be grounded.
4. If an existing codebase is the source of truth, read the actual product content and source files before designing. Prioritize README copy, config/schema terminology, CLI/help text, stylesheets, theme files, Tailwind config, CSS custom properties, and component files. If a running app or screenshots are available, compare the draft DESIGN.md against the rendered UI and revise until both tokens and prose match the product's visual identity.
5. If a Stitch project already exists, retrieve the active design system and screen styling where available, then synthesize the same token/prose model into DESIGN.md.
6. If no Stitch project, codebase, brand reference, or user-provided direction exists, stop and ask for design input. Do not synthesize a design system from empty placeholders.
7. Draft DESIGN.md and request user approval before prototype prompt generation.
8. Validate front matter, token references, duplicate section headings, and required screen-specific design coverage.

### DESIGN.md update workflow

1. Identify the change trigger (new platform, new component, rebrand, design review, Stitch panel edit, etc.).
2. Scope affected token groups and markdown sections.
3. Update tokens and prose together so normative values and rationale do not conflict.
4. Preserve unknown custom tokens and sections unless they duplicate canonical sections or contradict active values.
5. If using Stitch, upload or register the updated DESIGN.md as the project design system when needed. Set it as the project default only when future screens should inherit it.
6. Report which token groups and markdown sections changed so downstream design artifacts can be checked for compatibility.
