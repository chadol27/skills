# Forms: Switch

Official docs: `https://picocss.com/docs/forms/switch`

Pico builds switches from checkbox markup plus the `role="switch"` attribute.

## Key points

- The switch uses `<input type="checkbox" role="switch">`.
- Checked, disabled, and invalid states are supported.
- Group switches in a `<fieldset>` when they belong together.
- Validation uses `aria-invalid`.

## Syntax

```html
<fieldset>
  <label>
    <input name="terms" type="checkbox" role="switch" />
    I agree to the Terms
  </label>
  <label>
    <input name="opt-in" type="checkbox" role="switch" checked />
    Receive news and offers
  </label>
</fieldset>
```
