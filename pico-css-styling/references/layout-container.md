# Layout: Container

Official docs: `https://picocss.com/docs/container`

Use `.container` for a centered fixed-width viewport and `.container-fluid` for a full-width layout.

## Key points

- Pico defines six default container breakpoints and scales the viewport width across device sizes.
- `.container` creates a centered readable column.
- `.container-fluid` stretches to the full available width.
- These classes are not available in the class-less build.
- In the class-less build, `<header>`, `<main>`, and `<footer>` act as semantic containers instead.

## Syntax

```html
<main class="container">
  <h1>Page</h1>
</main>

<main class="container-fluid">
  <h1>Full width page</h1>
</main>
```
