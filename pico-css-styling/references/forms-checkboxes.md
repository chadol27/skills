# Forms: Checkboxes

Official docs: `https://picocss.com/docs/forms/checkboxes`

Pico gives native checkboxes a responsive custom style while keeping semantic form markup intact.

## Key points

- Use `<fieldset>` and `<legend>` for grouped checkbox sets.
- Standard label-wrapped checkbox markup is the default pattern.
- Horizontal stacking is supported by placing inputs and labels as siblings.
- Indeterminate state is set with JavaScript by toggling the element's `indeterminate` property.
- Validation states use `aria-invalid`.

## Syntax

```html
<fieldset>
  <legend>Language preferences</legend>
  <label>
    <input type="checkbox" name="english" checked />
    English
  </label>
  <label>
    <input type="checkbox" name="thai" />
    Thai
  </label>
</fieldset>
```
