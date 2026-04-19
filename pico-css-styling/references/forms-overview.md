# Forms: Overview

Official docs: `https://picocss.com/docs/forms`

Pico's form system is semantic-first and keeps all native form controls responsive by default.

## Key points

- Form controls are full width by default and match button sizing.
- Inputs can live either inside the related `<label>` or after a `for`-linked label.
- `<small>` under a field acts as muted helper text.
- `.grid` can be used inside forms for quick multi-column form rows.
- `role="group"` can be used to visually merge controls in the same row.

## Syntax

```html
<form>
  <fieldset>
    <label>
      First name
      <input name="first_name" placeholder="First name" />
    </label>

    <label for="email">Email</label>
    <input id="email" type="email" placeholder="Email" />
    <small>We'll never share your email.</small>
  </fieldset>

  <input type="submit" value="Subscribe" />
</form>
```
