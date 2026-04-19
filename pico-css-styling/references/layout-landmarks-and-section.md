# Layout: Landmarks & Section

Official docs: `https://picocss.com/docs/landmarks-section`

Use semantic landmarks to structure the page and let Pico apply responsive spacing with minimal extra markup.

## Key points

- `<header>`, `<main>`, and `<footer>` as direct children of `<body>` get responsive vertical padding.
- `<section>` provides responsive bottom spacing between content blocks.
- This pattern is part of Pico's semantic-first layout approach.
- If the app root is not `<body>`, Pico can be recompiled with a custom semantic root selector.

## Syntax

```html
<body>
  <header>...</header>
  <main>
    <section>...</section>
    <section>...</section>
  </main>
  <footer>...</footer>
</body>
```

## Notes

- The custom root container setup belongs to Pico's Sass recompilation workflow.
- For normal usage, prefer the default semantic landmarks before adding wrapper classes.
