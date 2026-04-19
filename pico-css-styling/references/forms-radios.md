# Forms: Radios

Official docs: `https://picocss.com/docs/forms/radios`

Pico styles native radio inputs with the same semantic approach used across the rest of the form system.

## Key points

- Use `<fieldset>` and `<legend>` for radio groups.
- Label-wrapped markup is the default pattern.
- Horizontal stacking is supported with sibling input and label pairs.
- Validation states use `aria-invalid`.
- Disabled radio options can be paired with `aria-disabled="true"` on the label.

## Syntax

```html
<fieldset>
  <legend>Language preference</legend>
  <label>
    <input type="radio" name="language" checked />
    English
  </label>
  <label>
    <input type="radio" name="language" />
    French
  </label>
</fieldset>
```
