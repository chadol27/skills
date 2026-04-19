# Content: Button

Official docs: `https://picocss.com/docs/button`

Pico styles native buttons directly and adds a small variant surface for emphasis changes.

## Key points

- Default button styling comes from the native `<button>` element with no extra class.
- `.secondary` and `.contrast` provide alternate variants.
- `.outline` creates the outline style and can be combined with the other variants.
- `input[type="submit"]` and `input[type="button"]` are styled as buttons.
- `input[type="reset"]` gets the secondary style by default.
- Disabled buttons and `role="button"` elements are supported.

## Syntax

```html
<button>Save</button>
<button class="secondary">Cancel</button>
<button class="contrast">Delete</button>
<button class="outline">Preview</button>
<div role="button" tabindex="0">Div as button</div>
```
