# Components: Loading

Official docs: `https://picocss.com/docs/loading`

Pico uses `aria-busy="true"` to display loading indicators without adding custom loader components.

## Key points

- `aria-busy="true"` works on blocks, inline elements, and buttons.
- Loading can be combined with button variants like `.secondary`, `.contrast`, and `.outline`.
- Use `aria-label` when the visible label is omitted.

## Syntax

```html
<article aria-busy="true"></article>
<span aria-busy="true">Generating your link...</span>
<button aria-busy="true">Please wait…</button>
```
