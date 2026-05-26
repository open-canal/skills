# Design Standard

## DESIGN.md — Project Design System

A project-level `DESIGN.md` is required as the semantic design system source of truth. It serves as the visual source of truth for all prototype prompts and generated screens.

The specification is Stitch-compatible. When the target prototype tool is Figma or another tool, DESIGN.md still serves as the design reference but §6 (Stitch-specific notes) is optional.

Reference specification: [Stitch DESIGN.md overview](https://stitch.withgoogle.com/docs/design-md/overview)

### Location

```text
.open-canal/design-standards/DESIGN.md
```

### Required Sections

Every `DESIGN.md` must begin with YAML frontmatter containing only `tags`:

```yaml
---
tags:
  - design-standard
  - design-md
---
```

The body must include:

| Section | Content |
|---|---|
| **1. Visual Theme & Atmosphere** | Mood adjectives, density, aesthetic philosophy. Use evocative natural language (e.g. "sophisticated minimalist sanctuary", "airy yet grounded"). |
| **2. Color Palette & Roles** | Descriptive name + hex code + functional role per color. Primary background, surface, accent/CTA, text hierarchy, functional states (success/warning/error/info). |
| **3. Typography Rules** | Font family, weight usage per hierarchy level (H1–H4, body, caption, CTA), letter-spacing, line-height, spacing principles. |
| **4. Component Stylings** | Buttons (shape, color, hover/focus/disabled states), cards/containers (corner style, background, shadow strategy, internal padding), navigation, inputs/forms, and any project-specific patterns. Always translate CSS values to natural language (e.g. "subtly rounded corners" not "rounded-md", "whisper-soft diffused shadows" not "shadow-sm"). |
| **5. Layout Principles** | Grid system, max content width, breakpoints, whitespace strategy (base unit, section margins, edge padding), alignment, responsive behavior, touch target minimums. |
| **6. Design System Notes for Stitch** | _(Required for Stitch; optional for Figma)_ Language to use in Stitch prompts, color references with descriptive names + hex codes, component prompt templates, incremental iteration guidance. |

### DESIGN.md Language Rules

- Use descriptive design terminology. Avoid raw CSS class names in descriptions.
- Include exact hex codes in parentheses after descriptive color names: `"Deep Muted Teal-Navy (#294056)"`.
- Describe geometry physically: `"generously rounded corners"` not `"rounded-xl"`, `"pill-shaped"` not `"rounded-full"`.
- Describe depth qualitatively: `"whisper-soft diffused shadows"`, `"flat — no elevation"`, `"heavy high-contrast drop shadows"`.
- Explain the functional role of every design element, not just its appearance.

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
- **project `.open-canal/design-standards/DESIGN.md`** — required; when missing, fail early by switching to the DESIGN.md creation workflow (§ DESIGN.md Maintenance) and requesting user approval before any prototype prompt generation;
- existing `design.md` when updating;
- project `.open-canal/design-standards/` for additional style constraints;
- linked version file when version scope changes the screen set.

### DESIGN.md Prerequisite Gate

Before generating any prototype prompt, verify that `.open-canal/design-standards/DESIGN.md` exists and covers all required sections from [DESIGN.md Required Sections](#required-sections).

| State | Action |
|-------|--------|
| **Missing** | Switch to DESIGN.md creation workflow (§ DESIGN.md Maintenance). Synthesize from available inputs. Request user approval. Do not proceed with prototype prompts until DESIGN.md is approved. |
| **Incomplete** | Report missing sections to the user with specific gaps. Offer to update DESIGN.md to fill them. For Figma-targeted projects, §6 is optional; all other sections are required regardless of tool. Do not proceed with prototype prompts until all required sections are complete. |
| **Complete** | Proceed to prototype prompt generation. Quote DESIGN.md sections directly. |

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
- **DESIGN.md reference** — link to `.open-canal/design-standards/DESIGN.md` and confirm it was loaded;
- **screen list** — every screen the user will see, one row per screen;
- **state matrix** — a table of screen × state (Normal, Loading, Empty, Error, Permission Denied) with a short description per cell;
- **UI element checklist** — per screen, list every interactive and display element;
- **storyboard order** — numbered sequence of frames in user-action order;
- **interaction details** — trigger → response pairs per interaction;
- style constraints from `.open-canal/design-standards/` and DESIGN.md;
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
- **visual style constraints from DESIGN.md** — quote DESIGN.md sections directly for color names, typography rules, component styling descriptions, and atmosphere language;
- **component prompt templates** from DESIGN.md §6 when the requirement's UI elements match documented components;
- target platform and viewport;
- resource/output location expectations.

For interactive requirements, output frames in user-action order: entry, pre-action, feedback/loading/confirmation/permission, result, cancel/back, and failure recovery.

## Visual Rules

- One frame shows exactly one page and one state.
- Colors use descriptive names + hex codes from DESIGN.md §2. Do not use raw hex codes without their DESIGN.md semantic name.
- Typography follows DESIGN.md §3 hierarchy and spacing.
- Component styling follows DESIGN.md §4 for all reusable UI elements. Document deviations explicitly when requirement-specific needs override.
- Navigation follows DESIGN.md §5 layout principles and target platform conventions.
- Touch targets meet the target platform accessibility minimum as stated in DESIGN.md §5.
- When DESIGN.md is missing or incomplete, alert the user and pause. Do not substitute with generic design tokens.
- Avoid unrelated platform visual language, low-contrast text, inconsistent spacing, and custom gestures without fallback.

## Update Rules

When updating design after PRD changes:

1. Compare old and new PRD behavior. List every user-visible change before editing the design.
2. Check DESIGN.md for staleness — if new PRD behavior requires design tokens not in DESIGN.md, report the gap.
3. Update only affected screens, states, and prompt sections.
4. Keep previous prototype asset links when still valid. An asset is still valid when the screen layout, state, and key interactions it represents have not changed.
5. Mark stale assets in the `design.md` Stale Assets section. A stale asset is any prototype file that no longer matches the updated PRD behavior. Include the file path and reason.
6. Report the stale asset list alongside the updated design artifact.

## DESIGN.md Maintenance

### When to create or update DESIGN.md

- **Create** when `.open-canal/design-standards/DESIGN.md` does not exist and the project has at least one screen to generate.
- **Update** when:
  - A new platform or viewport is targeted.
  - PRD changes introduce new component patterns not documented in DESIGN.md.
  - Visual style constraints change (rebrand, design tokens update).
  - A design review reveals inconsistent generated output that should be codified.

The `design` workflow must always target a specific demand PRD, and on create/update it must update both the `design.md` artifact and the source PRD's design link.

### DESIGN.md creation workflow

1. Add YAML frontmatter with `tags: [design-standard, design-md]`.
2. Determine the target prototype tool (Stitch or Figma) from the PRD or user input. §6 is required only for Stitch.
3. If a Stitch project already exists: retrieve screen metadata via MCP, extract design tokens from HTML/CSS, and synthesize into DESIGN.md following the [Required Sections](#required-sections).
4. If no Stitch project exists: gather constraints from PRD platform, `.open-canal/design-standards/`, and user-provided design references. **If all four design-standards skeleton files contain only placeholder comments and no user-provided references exist, stop and ask the user for design input.** Do not synthesize a design system from empty placeholders.
5. Draft DESIGN.md and request user approval before prototype prompt generation.
6. Validate that every required section is present and uses the descriptive language rules defined in [DESIGN.md Language Rules](#designmd-language-rules).

### DESIGN.md update workflow

1. Identify the change trigger (new platform, new component, rebrand, etc.).
2. Scope the affected DESIGN.md sections.
3. Update only the changed sections. Keep the descriptive language style consistent.
4. Report which DESIGN.md sections changed so downstream design artifacts can be checked for compatibility.
