# Cybercore CSS Reference

Source: https://sebyx07.github.io/cybercore-css/llm.txt

Cybercore CSS is a pure CSS cyberpunk framework with no JavaScript dependency. It is optimized for AI-assisted use and exposes framework classes with a `cyber-` prefix.

## Installation

Install from npm:

```bash
npm install cybercore-css
```

Import the package in the app's main stylesheet or entry point according to the framework's conventions. For SCSS codebases, prefer modular imports when only some Cybercore modules are needed.

## Modular SCSS Imports

Core modules:

```scss
@use 'cybercore-css/core/variables';
@use 'cybercore-css/core/reset';
@use 'cybercore-css/core/base';
```

Components:

```scss
@use 'cybercore-css/components/buttons';
@use 'cybercore-css/components/cards';
@use 'cybercore-css/components/inputs';
@use 'cybercore-css/components/alerts';
@use 'cybercore-css/components/badges';
@use 'cybercore-css/components/tabs';
@use 'cybercore-css/components/modal';
@use 'cybercore-css/components/dropdown';
@use 'cybercore-css/components/tables';
@use 'cybercore-css/components/terminal';
@use 'cybercore-css/components/progress';
@use 'cybercore-css/components/spinner';
@use 'cybercore-css/components/skeleton';
```

Effects:

```scss
@use 'cybercore-css/effects/glitch';
@use 'cybercore-css/effects/neon-border';
@use 'cybercore-css/effects/scanlines';
@use 'cybercore-css/effects/datastream';
```

Utilities:

```scss
@use 'cybercore-css/utilities/display';
@use 'cybercore-css/utilities/flex';
@use 'cybercore-css/utilities/grid';
@use 'cybercore-css/utilities/spacing';
@use 'cybercore-css/utilities/text';
@use 'cybercore-css/utilities/position';
```

## Naming

- Base classes: `.cyber-btn`, `.cyber-card`
- Modifiers: `.cyber-btn--magenta`, `.cyber-btn--ghost`
- Elements: `.cyber-card__header`, `.cyber-card__body`

## Color Tokens

Cybercore CSS uses CSS variables with 100-900 scales:

- `--cyber-cyan-*`: primary
- `--cyber-magenta-*`: danger
- `--cyber-yellow-*`: warning
- `--cyber-green-*`: success
- `--cyber-void-*`: dark backgrounds
- `--cyber-chrome-*`: grays

## Components

- Buttons: `.cyber-btn` with `--magenta`, `--yellow`, `--green`, `--ghost`, `--sm`, `--lg`
- Cards: `.cyber-card` with `__header`, `__body`, `__footer`, `--interactive`, `--holo`
- Inputs: `.cyber-input`, `.cyber-textarea`, `.cyber-checkbox`, `.cyber-field`
- Alerts: `.cyber-alert` with `--info`, `--success`, `--warning`, `--error`
- Badges: `.cyber-badge` with color variants and `--pulse`
- Tabs: `.cyber-tabs`, `.cyber-tabs__tab`, `--active`
- Modal: `.cyber-modal`, `__backdrop`, `__content`, `__header`, `__body`, `__footer`
- Dropdown: `.cyber-dropdown`, `__trigger`, `__menu`, `__item`, `--right`, `--up`
- Tables: `.cyber-table` with `--striped`, `--compact`
- Terminal: `.cyber-terminal`, `__header`, `__body`
- Progress: `.cyber-progress` with the `--value` CSS property
- Spinner: `.cyber-spinner` with size and color variants
- Skeleton: `.cyber-skeleton`, `--text`, `--block`

## Effects

- Glitch: `.cyber-glitch` with a `data-text` attribute for chromatic aberration
- Neon border: `.cyber-neon-border` with `--cyan`, `--magenta`, `--yellow`, `--green`
- Text glow: `.cyber-text-glow` with color variants
- Scanlines: `.cyber-scanlines` for a CRT overlay
- Datastream: `.cyber-datastream` animated gradient

## Utilities

Display:

`.cyber-hidden`, `.cyber-block`, `.cyber-inline`, `.cyber-inline-block`, `.cyber-flex`, `.cyber-inline-flex`, `.cyber-grid`

Flexbox:

`.cyber-flex-col`, `.cyber-flex-row`, `.cyber-flex-wrap`, `.cyber-items-center`, `.cyber-items-start`, `.cyber-items-end`, `.cyber-justify-center`, `.cyber-justify-between`, `.cyber-justify-around`, `.cyber-flex-1`, `.cyber-flex-auto`

Grid:

`.cyber-grid--2`, `.cyber-grid--3`, `.cyber-grid--4`, `.cyber-grid--auto-sm`, `.cyber-grid--auto-md`, `.cyber-grid--auto-lg`, `.cyber-col-span-2`, `.cyber-col-span-full`

Spacing:

`.cyber-m-{size}`, `.cyber-mx-{size}`, `.cyber-my-{size}`, `.cyber-mt-{size}`, `.cyber-mb-{size}`, `.cyber-p-{size}`, `.cyber-px-{size}`, `.cyber-py-{size}`, `.cyber-gap-{size}`

Supported spacing sizes: `0`, `xs`, `sm`, `md`, `lg`, `xl`, `auto`

Text:

`.cyber-text-left`, `.cyber-text-center`, `.cyber-text-right`, `.cyber-text-primary`, `.cyber-text-muted`, `.cyber-text-cyan`, `.cyber-text-magenta`, `.cyber-text-yellow`, `.cyber-text-green`, `.cyber-font-bold`, `.cyber-font-mono`, `.cyber-uppercase`, `.cyber-truncate`

Position:

`.cyber-relative`, `.cyber-absolute`, `.cyber-fixed`, `.cyber-sticky`, `.cyber-inset-0`, `.cyber-top-0`, `.cyber-right-0`, `.cyber-bottom-0`, `.cyber-left-0`, `.cyber-center-xy`

## Architecture

Cybercore CSS uses CSS layers for cascade control:

```css
@layer reset, base, theme, components, utilities;
```

Utilities are intended to override components.

## Icons

Cybercore CSS includes 153 cyberpunk SVG icons across 10 categories. Icons are stroke-based and use `currentColor`.

Standard JS/TS import:

```typescript
import { renderIcon, getIcon, icons } from 'cybercore-css/icons';

const html = renderIcon('terminal', {
  size: 24,
  color: 'cyan',
  variant: 'outline'
});

const svg = getIcon('terminal');
```

Tree-shakeable JS/TS import:

```typescript
import { renderIconDef, getIconSvg } from 'cybercore-css/icons';
import { terminal, chip } from 'cybercore-css/icons/defs/tech';
import { arrowUp } from 'cybercore-css/icons/defs/navigation';

const html = renderIconDef(terminal, { size: 24, color: 'cyan' });
const chipHtml = renderIconDef(chip, { size: 32, variant: 'solid' });
const svg = getIconSvg(arrowUp);
const solidSvg = getIconSvg(terminal, 'solid');
```

For HTML-only usage, copy SVGs from `node_modules/cybercore-css/src/icons/svg/` or inline trusted SVG markup.

Icon options:

- Sizes: `16`, `20`, `24`, `32`, `48`, `64`
- Colors: `cyan`, `magenta`, `yellow`, `green`, `current`
- Variants: `outline`, `solid`, `duotone`, `glitch`

Icon CSS classes:

- Color: `.cyber-icon--cyan`, `.cyber-icon--magenta`, `.cyber-icon--yellow`, `.cyber-icon--green`
- Size: `.cyber-icon--sm` (16px), `.cyber-icon--md` (24px), `.cyber-icon--lg` (32px), `.cyber-icon--xl` (48px)
- Animation: `.cyber-icon--spin`, `.cyber-icon--pulse`, `.cyber-icon--glitch`

## Icon Categories

- Navigation (14): arrow-up, arrow-down, arrow-left, arrow-right, chevron-up, chevron-down, chevron-left, chevron-right, home, menu, menu-dots, external-link, maximize, minimize
- Actions (23): search, filter, sort, edit, delete, copy, paste, cut, download, upload, share, save, undo, redo, plus, minus, check, x, refresh, drag, pin, link, unlink
- Media (17): play, pause, stop, skip-back, skip-forward, rewind, fast-forward, volume-high, volume-low, volume-off, mic, mic-off, camera, camera-off, image, video, music
- Communication (10): bell, bell-off, message, mail, inbox, send, at-sign, phone, phone-off, video-call
- Data (14): chart-bar, chart-line, chart-pie, database, table, cloud, cloud-upload, cloud-download, sync, calendar, clock, timer, hash, percent
- Security (15): lock, unlock, key, shield, shield-check, shield-x, eye, eye-off, fingerprint, user, users, user-plus, user-minus, log-in, log-out
- Tech (22): terminal, code, chip, circuit, wifi, wifi-off, bluetooth, server, api, git-branch, git-commit, git-merge, git-pull, bug, settings, sliders, zap, cpu, memory, globe, qr-code, signal
- Files (15): file, file-text, file-code, file-image, file-video, file-audio, file-archive, file-plus, file-minus, folder, folder-open, folder-plus, archive, clipboard, attachment
- Status (12): info, warning, error, success, help, loading, progress, online, offline, battery-full, battery-low, battery-charging
- Social (11): heart, heart-filled, star, star-filled, thumbs-up, thumbs-down, bookmark, bookmark-filled, flag, award, trending

## Accessibility

- Respect `prefers-reduced-motion`.
- Maintain WCAG AA contrast.
- Use `.cyber-sr-only` for screen-reader-only text.
- Add labels or accessible names for icon-only controls.

## Related Links

- GitHub: https://github.com/sebyx07/cybercore-css
- Demo: https://sebyx07.github.io/cybercore-css
- NPM: https://www.npmjs.com/package/cybercore-css
- Companion chart library: https://github.com/sebyx07/cybercore-charts
