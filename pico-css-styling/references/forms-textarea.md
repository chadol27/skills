# Forms: Textarea

Official docs: `https://picocss.com/docs/forms/textarea`

Pico styles `<textarea>` to match the rest of the input family.

## Key points

- Textareas follow the same visual language as text inputs.
- `disabled` and `readonly` states are supported.
- Validation uses `aria-invalid`.
- Helper text in `<small>` inherits the validation color just like with other controls.

## Syntax

```html
<textarea
  name="bio"
  placeholder="Write a professional short bio..."
  aria-label="Professional short bio"
></textarea>

<textarea name="invalid" aria-invalid="true">Invalid</textarea>
<small>Please provide a valid value.</small>
```
