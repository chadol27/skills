# Content: Typography

Official docs: `https://picocss.com/docs/typography`

Pico styles typography responsively with semantic HTML and keeps the scale consistent across devices.

## Key points

- Base text and heading sizes scale with viewport size.
- Heading levels `h1` through `h6` already have responsive sizing.
- `<hgroup>` collapses heading spacing and mutes the last child.
- Inline text elements like `strong`, `em`, `cite`, `mark`, `kbd`, `small`, `sub`, and `sup` are styled out of the box.
- `<blockquote>` and `<hr>` also have documented native styling.

## Syntax

```html
<hgroup>
  <h2>Get inspired with CSS</h2>
  <p>How to use CSS to add glam to your website?</p>
</hgroup>

<blockquote>
  Quote text
  <footer><cite>Author</cite></footer>
</blockquote>
```

## Notes

- Prefer semantic tags over utility classes for text emphasis.
- Pico's typography is meant to work with default HTML structure first.
