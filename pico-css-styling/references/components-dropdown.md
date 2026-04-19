# Components: Dropdown

Official docs: `https://picocss.com/docs/dropdown`

Pico builds dropdowns with semantic details markup and no JavaScript requirement.

## Key points

- Use `<details class="dropdown">` as the wrapper.
- Use `<summary>` as the toggle and a direct child `<ul>` for menu items.
- `summary[role="button"]` turns the trigger into a button-like control.
- Button variants like `.secondary`, `.contrast`, and `.outline` are supported on dropdown triggers.
- Dropdown is not available in the class-less build.

## Syntax

```html
<details class="dropdown">
  <summary>Options</summary>
  <ul>
    <li><a href="#">Profile</a></li>
    <li><a href="#">Billing</a></li>
  </ul>
</details>
```
