# Forms: Input

Official docs: `https://picocss.com/docs/forms/input`

Pico styles native input types consistently and supports validation and state styling with standard HTML attributes.

## Key points

- Common types like `text`, `email`, `number`, `password`, `tel`, and `url` share the same base look.
- Date and time related inputs come with matching Pico treatment.
- `type="search"` has its own distinctive style.
- `type="color"` and `type="file"` are also supported.
- `disabled`, `readonly`, and `aria-invalid` control state styling.
- Helper text in `<small>` inherits validation state color when connected to the field.

## Syntax

```html
<input type="email" name="email" placeholder="Email" autocomplete="email" />
<input type="search" name="search" placeholder="Search" />
<input type="file" />
<input type="text" value="Invalid" aria-invalid="true" />
```
