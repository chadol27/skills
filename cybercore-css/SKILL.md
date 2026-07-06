---
name: cybercore-css
description: Use when building or styling web interfaces with the Cybercore CSS cyberpunk CSS framework, including Svelte/SvelteKit, HTML, SCSS, component class selection, modular imports, utility classes, Cybercore icons, and cyberpunk visual effects from the cybercore-css npm package.
---

# Cybercore CSS

## Overview

Use Cybercore CSS to build cyberpunk-themed interfaces with pure CSS, no JavaScript dependency, and `cyber-` prefixed BEM-style classes. Prefer Cybercore's components, effects, utilities, CSS variables, and icon APIs before inventing custom cyberpunk styles.

## Workflow

1. Check whether `cybercore-css` is installed in the project. If not, add it with npm or adapt to the local package source if the repo vendors Cybercore CSS.
2. Choose the lightest import approach that fits the codebase:
   - Use the package entry point when the app wants the full framework.
   - Use SCSS modular imports when tree size and cascade control matter.
3. Read [references/cybercore-css.md](references/cybercore-css.md) before choosing classes, icons, color tokens, or imports.
4. Compose UI with Cybercore classes first. Add local CSS only for layout, product-specific spacing, or behavior that Cybercore does not cover.
5. Preserve accessibility: include semantic HTML, labels, focus states, reduced-motion awareness, and screen-reader text where icon-only controls are used.

## Implementation Guidance

- Use `cyber-` prefixed classes consistently and follow BEM-style modifiers/elements such as `.cyber-btn--magenta` and `.cyber-card__body`.
- Use color variables for theme work instead of hard-coded cyber colors where possible.
- Use effects such as glitch, neon border, scanlines, and datastream deliberately; keep dense app surfaces readable.
- For icons in JS/TS, prefer tree-shakeable imports from `cybercore-css/icons/defs/...` when only a few icons are needed.
- For Svelte, render Cybercore icons as sanitized/static SVG only when the source is trusted, or use direct SVG markup from the package. Avoid injecting user-controlled SVG strings.
- If working in a Svelte project, still follow the Svelte MCP workflow and run the Svelte autofixer after editing Svelte code.

## Reference

Read [references/cybercore-css.md](references/cybercore-css.md) for installation snippets, modular SCSS imports, component classes, utility classes, icon APIs, icon categories, accessibility notes, and official links.
