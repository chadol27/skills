# Components: Nav

Official docs: `https://picocss.com/docs/nav`

Pico's nav component is a semantic `<nav>` with list-based structure and a few documented variations.

## Key points

- Build navs with `<nav>` containing one or more `<ul>` lists.
- Lists distribute horizontally by default.
- Link variants like `.secondary`, `.contrast`, and `.outline` are supported.
- Buttons can be placed inside nav list items.
- Dropdowns can be nested inside nav.
- Inside `<aside>`, nav items stack vertically.
- `nav[aria-label="breadcrumb"]` turns the nav into a breadcrumb.
- Pico documents overflow behavior and breadcrumb divider customization with `--pico-nav-breadcrumb-divider`.

## Syntax

```html
<nav>
  <ul>
    <li><strong>Acme Corp</strong></li>
  </ul>
  <ul>
    <li><a href="#">About</a></li>
    <li><a href="#">Services</a></li>
    <li><button class="secondary">Products</button></li>
  </ul>
</nav>
```
