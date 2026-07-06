# Customization: Sass

Official docs: `https://picocss.com/docs/sass`

Use Sass when a project should compile a custom Pico build instead of overriding the distributed CSS. This is the right path for module selection, classless builds, theme color changes, CSS variable prefix changes, and custom themes.

## Key points

- Compile a custom Pico build when the project needs fewer modules or design-system-level settings.
- Keep Pico core files unchanged so future Pico updates stay straightforward.
- Prefer Sass configuration over broad CSS overrides for Pico-wide behavior.
- Use CSS overrides only for project-specific gaps after Pico is configured.

## Import

Use `@use` in the project SCSS entry.

```scss
@use "pico";
```

With the Sass CLI, set Pico's SCSS folder as a load path to avoid long relative imports.

```bash
sass --load-path=node_modules/@picocss/pico/scss/ src/styles.scss dist/styles.css
```

In React, Webpack, or `sass-loader` projects that resolve `node_modules`, import Pico directly from the package.

```scss
@use "@picocss/pico/scss/pico";
```

## Settings

Set Pico options with `@use ... with (...)`. Use this for classless builds, semantic containers, module filtering, transitions, responsive typography, responsive spacing, parent scoping, breakpoints, and CSS variable prefix changes.

Classless build:

```scss
@use "pico" with (
  $enable-semantic-container: true,
  $enable-classes: false
);
```

Lightweight build pattern:

```scss
@use "pico" with (
  $enable-semantic-container: true,
  $enable-classes: false,
  $modules: (
    "content/code": false,
    "forms/input-color": false,
    "forms/input-date": false,
    "forms/input-file": false,
    "forms/input-range": false,
    "forms/input-search": false,
    "components/accordion": false,
    "components/card": false,
    "components/dropdown": false,
    "components/loading": false,
    "components/modal": false,
    "components/nav": false,
    "components/progress": false,
    "components/tooltip": false,
    "utilities/accessibility": false,
    "utilities/reduce-motion": false
  )
);
```

Important setting families:

- `$theme-color`: choose Pico's primary theme color.
- `$css-var-prefix`: change the CSS variable prefix; it must start with `--`.
- `$semantic-root-element`: define the root used for semantic header, main, and footer container behavior.
- `$enable-semantic-container`: make semantic landmarks act as containers.
- `$enable-viewport`: center semantic containers in the viewport when semantic containers are enabled.
- `$enable-responsive-spacings`: make landmark, section, and article spacing responsive.
- `$enable-responsive-typography`: make root font size responsive.
- `$enable-classes`: disable to generate the classless build.
- `$enable-transitions`: enable or disable transitions.
- `$enable-important`: control whether Pico emits `!important`.
- `$parent-selector`: scope generated HTML tag styles under a parent selector; `:root` stays unwrapped.
- `$breakpoints`: customize viewport and root font-size breakpoints.
- `$modules`: enable or disable Pico modules.

## Theme Color

Pico's default theme color is `azure`. Recompile with another built-in primary color when the project needs a different Pico accent without a fully custom theme.

```scss
@use "pico" with (
  $theme-color: "purple"
);
```

Built-in color choices:

`amber`, `azure`, `blue`, `cyan`, `fuchsia`, `green`, `grey`, `indigo`, `jade`, `lime`, `orange`, `pink`, `pumpkin`, `purple`, `red`, `sand`, `slate`, `violet`, `yellow`, `zinc`.

## Custom Theme

Use a custom theme when the built-in theme color is not enough for the brand. Disable Pico's default theme module, then import the project theme after Pico.

```scss
@use "pico" with (
  $modules: (
    "themes/default": false
  )
);

@use "path/custom-theme";
```

Duplicate Pico's default theme as a starting point when building a custom theme, then edit the copied theme in the project rather than modifying Pico's source.
