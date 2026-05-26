---
tags:
  - project/design
  - doc/design
---

# Design: [Requirement Title]

## Source

PRD: [[./prd]]
DESIGN.md: [[.open-canal/design-standards/DESIGN.md]] _(loaded)_
Target tool: [Stitch | Figma]

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
- [ ] [Element 1 — e.g. title bar, button, input]
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

- [Interaction 1: trigger → response]
- [Interaction 2: trigger → response]

## Style Constraints

> All visual style MUST come from DESIGN.md. Quote specific DESIGN.md sections.

- **Atmosphere:** [from DESIGN.md §1 — e.g. "sophisticated minimalist sanctuary"]
- **Platform:** [iOS | Android | Web | Desktop]
- **Viewport:** [e.g. 390×844 for iPhone 14]
- **Colors:** Refer to DESIGN.md §2. Use descriptive names + hex codes per DESIGN.md (e.g. "Deep Muted Teal-Navy (#294056)").
- **Typography:** Refer to DESIGN.md §3. Quote font family, weight per hierarchy, letter-spacing, and line-height rules.
- **Components:** Refer to DESIGN.md §4. Quote button style, card styling, input styling, and any project-specific component descriptions.
- **Layout:** Refer to DESIGN.md §5. Quote grid, spacing, alignment, and touch target rules.
- **Accessibility:** [contrast ratio per DESIGN.md §2, scalable text per DESIGN.md §3]

## Assets

- Prototype: `assets/prototype/`
- References: `assets/references/`

## Stale Assets

> Update after design changes: list removed or outdated prototype files here.

- _(none)_

## Acceptance Notes

- [ ] [Note 1 for prototype review — e.g. "Screen X loading state matches PRD empty flow"]
- [ ] [Note 2]
