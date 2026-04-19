# Layout: Overflow Auto

Official docs: `https://picocss.com/docs/overflow-auto`

Use `.overflow-auto` when content can exceed the width of its container and should scroll instead of breaking the layout.

## Key points

- `.overflow-auto` enables automatic scrollbars only when content exceeds the element's bounds.
- Pico documents it mainly for responsive tables.
- This is the first fix to try before rewriting wide content into another pattern.

## Syntax

```html
<div class="overflow-auto">
  <table>
    ...
  </table>
</div>
```
