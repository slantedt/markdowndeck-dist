# Slide layouts

Every slide uses a layout that decides how its content is arranged — a centered title, a two-column split, a full-bleed image, and so on. This page covers how to set a layout, the eight built-in layouts, multi-column content, and speaker notes.

## Choosing a layout

Set a slide's layout with an HTML comment as the **first non-blank line** of the slide:

```markdown
<!-- layout: cover -->

# My talk
## A subtitle
```

The directive line is stripped from the rendered output, so it never shows on the slide. A few rules to keep in mind:

- The name is matched case-sensitively by the pattern `<!-- layout: <name> -->` (whitespace around the colon is flexible).
- An unknown name falls back to `default` and logs a warning.
- A `<!-- layout: … -->` comment inside a fenced code block is treated as code, not a directive.

If a slide has no directive, it renders with the `default` layout.

## The layouts

There are eight built-in layouts. Use these exact names:

| Name | Use |
|---|---|
| `default` | single column with a heading bar — regular slides |
| `cover` | title/subtitle centered horizontally and vertically |
| `section` | section divider; centered |
| `two-cols` | two equal columns; the heading spans both; `2rem` gap |
| `image-left` | image left, text right |
| `image-right` | text left, image right |
| `full-image` | one image fills the slide (`object-fit: contain`) |
| `end` | closing slide; centered |

A few examples for the less obvious ones.

A title slide with `cover`:

```markdown
<!-- layout: cover -->

# Quarterly review
## Q2 2026
```

A side-by-side comparison with `two-cols` (the heading spans both columns):

```markdown
<!-- layout: two-cols -->

# Before and after

::: columns
::: column
Old workflow: manual, slow.
:::
::: column
New workflow: scripted, fast.
:::
:::
```

A full-bleed visual with `full-image` (the image is scaled to fit with `object-fit: contain`):

```markdown
<!-- layout: full-image -->

![Architecture diagram](/Users/me/talk/diagram.png)
```

## Columns

For multi-column content, use Pandoc-style fenced divs. Wrap the set in `::: columns` and put each column in its own `::: column`:

```markdown
::: columns
::: column
Left content
:::
::: column
Right content
:::
:::
```

The `.columns` container lays out its `.column` children with `repeat(auto-fit, minmax(0, 1fr))` and a `2rem` gap, so columns share the width evenly. This works on any layout, including inside `two-cols`.

## Speaker notes

Add speaker notes with a `::: notes … :::` fenced div anywhere in a slide:

```markdown
# Roadmap

- Ship v1
- Plan v2

::: notes
Remember to mention the beta timeline here.
:::
```

Notes are extracted from the slide — they are **not** shown when the deck is presented. Instead they are collected into the PDF's speaker-notes appendix when you export. See [PDF & image export](./06-pdf-export.md) for the appendix.

> [!NOTE]
> Only **top-level** `::: notes` blocks are extracted. A `::: notes` block nested inside another fenced div (for example inside `::: columns`) is not picked up as notes — keep notes at the top level of the slide.

## Where to next

- [Content blocks](./04-content-blocks.md) — diagrams, charts, tables, math, and admonitions you can drop into any layout.
- [Themes](./05-themes.md) — the `.layout-<name>` selectors a custom theme can style.
- [PDF & image export](./06-pdf-export.md) — how speaker notes feed the PDF appendix.
