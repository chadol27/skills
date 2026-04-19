# Forms: Select

Official docs: `https://picocss.com/docs/forms/select`

Pico styles the native `<select>` to stay visually consistent with other inputs.

## Key points

- Single select and `multiple` select are both supported.
- Disabled selects and validation states use native attributes.
- `aria-invalid` controls the invalid or valid visual state.
- Helper text can sit below the select with `<small>`.
- Pico also documents the `Dropdown` component as a custom-select-like alternative.

## Syntax

```html
<select name="favorite-cuisine" aria-label="Select your favorite cuisine..." required>
  <option selected disabled value="">Select your favorite cuisine...</option>
  <option>Italian</option>
  <option>Japanese</option>
</select>

<select multiple size="6" aria-label="Select your favorite snacks...">
  <option>Fruits</option>
  <option>Nuts</option>
</select>
```
