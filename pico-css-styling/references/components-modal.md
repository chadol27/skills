# Components: Modal

Official docs: `https://picocss.com/docs/modal`

Pico's modal is built on the native `<dialog>` element and an inner `<article>` container.

## Key points

- Use `<dialog>` as the wrapper and `<article>` for the modal body.
- A close button inside `<header>` can use `rel="prev"` for Pico's top-right close placement.
- `<footer>` content is right-aligned by default inside the modal.
- Pico does not ship JavaScript for modal interaction.
- Utility classes `.modal-is-open`, `.modal-is-opening`, and `.modal-is-closing` are documented for page-level modal state handling.
- These utility classes are not available in the class-less build.

## Syntax

```html
<dialog open>
  <article>
    <header>
      <button aria-label="Close" rel="prev"></button>
      <p><strong>Thank you</strong></p>
    </header>
    <p>Modal body</p>
    <footer>
      <button class="secondary">Cancel</button>
      <button>Confirm</button>
    </footer>
  </article>
</dialog>
```
