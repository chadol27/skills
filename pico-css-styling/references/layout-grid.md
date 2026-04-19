# Layout: Grid

Official docs: `https://picocss.com/docs/grid`

Use `.grid` for minimal responsive auto-layout columns.

## Key points

- `.grid` is Pico's lightweight layout helper for quick multi-column structure.
- Columns collapse intentionally on small devices under `768px`.
- Pico keeps this grid intentionally small and does not try to be a full utility grid system.
- `.grid` is not available in the class-less build.

## Syntax

```html
<div class="grid">
  <div>1</div>
  <div>2</div>
  <div>3</div>
  <div>4</div>
</div>
```

## When to use it

- Use it for simple dashboards, settings pages, and side-by-side blocks.
- If the layout needs ordering, offsets, or dense breakpoint control, add custom CSS instead of expecting Pico to provide a full grid framework.
