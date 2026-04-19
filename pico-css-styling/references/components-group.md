# Components: Group

Official docs: `https://picocss.com/docs/group`

Use Pico's group pattern when form controls or buttons should be visually merged into one horizontal unit.

## Key points

- `role="group"` stacks children horizontally.
- `role="search"` also stacks children horizontally and applies search-specific treatment.
- Grouped controls do not collapse like `.grid` on mobile.
- The group focus style relies on `:has()` and falls back gracefully when unsupported.
- The pattern is designed mainly for form controls and buttons.

## Syntax

```html
<form>
  <fieldset role="group">
    <input type="email" placeholder="Enter your email" />
    <input type="submit" value="Subscribe" />
  </fieldset>
</form>

<div role="group">
  <button>Day</button>
  <button>Week</button>
  <button>Month</button>
</div>
```
