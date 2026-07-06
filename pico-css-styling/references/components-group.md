# Components: Group

Official docs: `https://picocss.com/docs/group`

Use Pico's group pattern when form controls or buttons should be visually merged into one horizontal unit.

## Key points

- `role="group"` stacks children horizontally.
- When `role="group"` is used with `<form>`, the group is `width: 100%;`.
- `role="search"` also stacks children horizontally and applies search-specific treatment.
- Grouped controls do not collapse like `.grid` on mobile.
- The group focus style relies on `:has()` and falls back gracefully when unsupported.
- The pattern is designed mainly for form controls and buttons.

## Syntax

```html
<form>
  <fieldset role="group">
    <input name="email" type="email" placeholder="Enter your email" autocomplete="email" />
    <input type="submit" value="Subscribe" />
  </fieldset>
</form>

<form role="search">
  <input name="search" type="search" placeholder="Search" />
  <input type="submit" value="Search" />
</form>

<div role="group">
  <button>Day</button>
  <button>Week</button>
  <button>Month</button>
</div>

<div role="group">
  <button aria-current="true">Active</button>
  <button>Button</button>
  <button>Button</button>
</div>

<div role="group">
  <button>Button</button>
  <button class="secondary">Button</button>
  <button class="contrast">Button</button>
</div>
```
