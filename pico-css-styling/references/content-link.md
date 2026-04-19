# Content: Link

Official docs: `https://picocss.com/docs/link`

Pico styles links by default and supports small emphasis variants without introducing a large link utility system.

## Key points

- Plain `<a>` gives the default link style.
- `.secondary` and `.contrast` are available for alternate link emphasis.
- `aria-current` marks the active link state and visually matches hover treatment.
- `.secondary` and `.contrast` are not available in the class-less build.

## Syntax

```html
<a href="#">Primary</a>
<a href="#" class="secondary">Secondary</a>
<a href="#" class="contrast">Contrast</a>
<a href="#" aria-current="page">Active link</a>
```
