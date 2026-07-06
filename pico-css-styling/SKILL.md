---
name: pico-css-styling
description: Style pages and UI with Pico CSS using semantic HTML and minimal custom CSS. Use when Codex needs to build or restyle interfaces with Pico CSS, start from Pico's default element styling, structure layouts with containers, sections, and articles, style forms, tables, and navigation semantically, customize Pico with Sass or color utilities, or extend Pico without fighting the framework.
---

# Pico CSS Styling

## Overview

Use Pico CSS as a semantic-first styling layer. Start with Pico's default HTML element styling and only add stronger hooks after confirming semantic markup is not enough.

## Start With Pico

Import one Pico stylesheet first. Use a preset theme when that already matches the direction, or the base build when the project owns more of the theme.

```tsx
import "@picocss/pico/css/pico.min.css";
```

Use semantic HTML before adding classes. Prefer tags like `main`, `section`, `article`, `header`, `footer`, `nav`, `form`, `fieldset`, `button`, and `table` so Pico can style them with minimal extra work.

Choose classless structure first. Reach for extra hooks only when layout, emphasis, grouping, overflow, or state handling is clearly better than plain markup.

## What Pico Does Not Try To Be

Pico is not a giant utility framework. Do not expect a huge Tailwind-style class surface.

It is strongest when:
- the markup is semantic
- the layout is simple to medium complexity
- forms, content pages, dashboards, and CRUD UI matter more than bespoke component art direction

It is weaker when:
- every component needs unique visual choreography
- the design depends on many tiny spacing utilities
- the UI system is heavily component-token driven and not semantic-first

In those cases, use Pico for baseline styling and add narrow custom CSS on top.

## Add Custom CSS Carefully

Add custom CSS only after checking that Pico cannot already express the result with:
- semantic HTML
- built-in element styling
- a documented Pico pattern

When custom CSS is necessary:
- scope it narrowly
- prefer a single class on the smallest useful wrapper
- avoid resetting base element styles globally
- avoid deep selector chains
- avoid mixing multiple styling systems for the same component

Good reasons to add custom CSS:
- a layout Pico does not provide cleanly
- a brand treatment that needs a specific wrapper or accent
- animation, positioning, or interaction details outside Pico's scope
- responsive adjustments for a specific component

## Build Responsive UI Without Overengineering

Start from Pico's default responsive behavior. Most text, forms, tables, and containers already degrade decently.

Only add breakpoints when the layout actually breaks. Prefer fixing structure first, then spacing, then typography. Avoid desktop-first pixel tuning.

For dense interfaces, reduce complexity on mobile by stacking sections, collapsing non-essential controls, or turning side-by-side content into one-column flow.

## Official Reference Map

Use the files in `references/` as the primary local mirror of Pico's official docs for these sections. They are grouped by the official table of contents and intentionally exclude `Getting started`.

### Customization

- `Sass`: `references/customization-sass.md`
- `Colors`: `references/customization-colors.md`

### Layout

- `Container`: `references/layout-container.md`
- `Landmarks & section`: `references/layout-landmarks-and-section.md`
- `Grid`: `references/layout-grid.md`
- `Overflow auto`: `references/layout-overflow-auto.md`

### Content

- `Typography`: `references/content-typography.md`
- `Link`: `references/content-link.md`
- `Button`: `references/content-button.md`
- `Table`: `references/content-table.md`

### Forms

- `Overview`: `references/forms-overview.md`
- `Input`: `references/forms-input.md`
- `Textarea`: `references/forms-textarea.md`
- `Select`: `references/forms-select.md`
- `Checkboxes`: `references/forms-checkboxes.md`
- `Radios`: `references/forms-radios.md`
- `Switch`: `references/forms-switch.md`
- `Range`: `references/forms-range.md`

### Components

- `Accordion`: `references/components-accordion.md`
- `Card`: `references/components-card.md`
- `Dropdown`: `references/components-dropdown.md`
- `Group`: `references/components-group.md`
- `Loading`: `references/components-loading.md`
- `Modal`: `references/components-modal.md`
- `Nav`: `references/components-nav.md`
- `Progress`: `references/components-progress.md`
- `Tooltip`: `references/components-tooltip.md`

## Where To Look For Missing Information

If the exact class, attribute, or pattern is not described in the local references, check Pico's official docs first.

Use these doc areas:
- Docs home: overall structure and current table of contents
- Layout: page wrappers, sections, grid, and overflow behavior
- Content: typography, links, buttons, and tables
- Forms: overview plus each native control type
- Components: accordion, card, dropdown, group, loading, modal, nav, progress, and tooltip
- Customization: Sass compilation, module selection, theme color, custom themes, color utilities, and named color scales

Official docs: `https://picocss.com/docs`

When working from memory is risky, verify against the docs before introducing a Pico-specific pattern that is not already known to exist.

## Use a Simple Working Loop

Follow this order:
1. Write semantic HTML.
2. Apply Pico stylesheet.
3. Check whether Pico's defaults already solve most of the UI.
4. Pull the matching reference file from `references/` based on the official Pico docs section and page name.
5. If the project needs a smaller build, classless output, primary theme color changes, or a custom Pico theme, read `references/customization-sass.md` before writing overrides.
6. If the project needs Pico color utility classes, named color tokens, or Sass color variables, read `references/customization-colors.md` before adding color CSS.
7. Add narrowly scoped custom CSS only for gaps Pico does not cover.

## Output Expectations

When using this skill, produce UI that feels intentional but not overbuilt. Favor readable markup, Pico-native patterns, and a small styling surface area that another engineer can maintain fast.
