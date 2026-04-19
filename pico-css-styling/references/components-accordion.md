# Components: Accordion

Official docs: `https://picocss.com/docs/accordion`

Pico's accordion uses semantic HTML with no JavaScript requirement.

## Key points

- Use `<details>` and `<summary>` for each accordion item.
- Add the same `name` to related `<details>` elements when only one should stay open.
- The `open` attribute controls the initially opened item.

## Syntax

```html
<details name="faq" open>
  <summary>What is Pico?</summary>
  <p>A minimal CSS framework.</p>
</details>

<details name="faq">
  <summary>Does it need JavaScript?</summary>
  <p>Not for basic accordion behavior.</p>
</details>
```
