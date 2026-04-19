# Content: Table

Official docs: `https://picocss.com/docs/table`

Pico gives `<table>` a clean default style with consistent spacing and minimal chrome.

## Key points

- Start with plain semantic table markup.
- Pico documents color-scheme-aware table styling.
- `.striped` is available when alternating row emphasis improves readability.
- Wide tables should usually be wrapped with `.overflow-auto`.

## Syntax

```html
<table class="striped">
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Build</td>
      <td>Done</td>
    </tr>
  </tbody>
</table>
```
