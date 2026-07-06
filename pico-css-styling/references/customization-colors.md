# Customization: Colors

Official docs: `https://picocss.com/docs/colors`

Use Pico colors when a project needs Pico's named color palette, utility classes, or Sass color variables. The main Pico stylesheet does not include color utilities.

## Key points

- Pico provides named color families with shades from `50` to `950`.
- The main Pico CSS build excludes color utilities.
- Avoid shipping the full color utility stylesheet in production unless the project truly needs all color families and shades.
- Prefer Sass for production builds so only used color families, shades, and utility types are generated.
- Use color utilities for small, explicit accents; use theme color or custom theme configuration for broad brand changes.

## Color Families

Pico's documented color families are:

`red`, `pink`, `fuchsia`, `purple`, `violet`, `indigo`, `blue`, `azure`, `cyan`, `jade`, `green`, `lime`, `yellow`, `amber`, `pumpkin`, `orange`, `sand`, `grey`, `zinc`, `slate`.

Each family uses shade values:

`50`, `100`, `150`, `200`, `250`, `300`, `350`, `400`, `450`, `500`, `550`, `600`, `650`, `700`, `750`, `800`, `850`, `900`, `950`.

## Usage With CSS

Only link the color utility stylesheet when the app can afford the extra CSS weight or is a prototype.

```html
<link rel="stylesheet" href="css/pico.colors.min.css">
```

CDN option:

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.colors.min.css"
>
```

After loading the color utilities, use `pico-color-{family}-{shade}` for text color and `pico-background-{family}-{shade}` for backgrounds.

```html
<h2 class="pico-color-pink-500">Pink title</h2>

<article class="pico-background-pink-600">
  Pink card
</article>
```

## Usage With Sass

Import all Pico colors as Sass variables when writing project SCSS.

```scss
@use "colors" as *;

h2 {
  color: $pink-500;
}
```

Generate color utility classes from Sass when utility classes are needed.

```scss
@use "colors/utilities";
```

Generate a smaller utility build by limiting families, shades, or utility types.

```scss
@use "colors/utilities" with (
  $palette: (
    "color-families": (
      red,
      pink,
      fuchsia,
      purple,
    ),
  ),
  $utilities: (
    "background-colors": false,
  )
);
```

Important setting families:

- `$css-var-prefix`: CSS variable prefix; it must start with `--`.
- `$css-class-prefix`: class prefix, `pico-` by default.
- `$palette`: control color families, shades, main colors, black and white export, light and dark companion colors, and export format.
- `$properties`: rename generated `color` and `background-color` property labels.
- `$utilities`: control CSS variables, text color utilities, background color utilities, and automatic text color for background utilities.

Use `"export-as": "hex"`, `"rgb"`, or `"hsl"` in `$palette` when the project needs a specific output format.

## Practical Guidance

For one-off brand accents, use Sass variables in scoped custom CSS instead of enabling a broad utility sheet.

For reusable color utility classes, generate the narrowest Sass utility set that covers the design.

For a global primary color, prefer `$theme-color` in `references/customization-sass.md` instead of scattering color utility classes across the UI.
