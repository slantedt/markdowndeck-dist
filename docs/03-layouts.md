# Slide layouts

Every slide is laid out on a **CSS grid**. A layout decides how many columns that grid has and what each one is for — a single centered column for a title, two columns for a comparison, an image column beside a text column. This page covers how to set a layout, the eight built-in layouts, how to place content into named columns with cells, defining your own grid, and speaker notes.

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

There are eight built-in layouts. Each is a **preset** — a named set of grid columns over one shared grid engine. Use these exact names:

| Name | Columns | Use |
|---|---|---|
| `default` | one column | regular slides with a heading bar |
| `cover` | one column, centered | title/subtitle centered horizontally and vertically |
| `section` | one column, centered | section divider |
| `two-cols` | `left` \| `right` (equal) | two equal columns; a heading spans both; `2rem` gap |
| `image-left` | `image` \| `text` | image column on the left, text on the right |
| `image-right` | `text` \| `image` | text on the left, image column on the right |
| `full-image` | one column | one image fills the slide (`object-fit: contain`) |
| `end` | one column, centered | closing slide |

`image-left` and `image-right` use the **same** column names (`image`, `text`) — they differ only in the order, so the `image` cell lands on the left or the right respectively from identical markup.

A title slide with `cover`:

```markdown
<!-- layout: cover -->

# Quarterly review
## Q2 2026
```

A full-bleed visual with `full-image` (the image is scaled to fit with `object-fit: contain`):

```markdown
<!-- layout: full-image -->

![Architecture diagram](/Users/me/talk/diagram.png)
```

## Placing content into columns with cells

A multi-column layout has **named columns**. To put content in a specific column, wrap it in a `::: cell <name> … :::` fenced div. The cell is placed in the column with that name **regardless of where it appears in the source**:

```markdown
<!-- layout: image-left -->

::: cell image
![Diagram](/Users/me/talk/diagram.png)
:::

::: cell text
## How it works

- step one
- step two
:::
```

Here the `image` cell goes in the left column and the `text` cell in the right — because that's how `image-left` orders its columns. Switch the directive to `image-right` and the *same markup* mirrors: the image moves to the right column. You don't reorder anything in the source.

Rules for cells:

- A `::: cell <name>` whose name matches a column is placed in that column.
- An **unnamed** `::: cell`, or a cell whose name matches no column on this slide, auto-flows into the next free column (and the CLI emits a `--json` warning for an unmatched name, so you can catch typos).
- Content that is **not** in a cell (a bare heading or paragraph) spans all columns — which is exactly what you want for a title above a two-column body.
- More cells than columns wrap onto a new row.

> [!NOTE]
> Letting bare top-level blocks flow into columns by themselves is **deprecated**. On a multi-column layout, put column content in `::: cell` blocks (or `::: columns`, below). A bare block now spans the full width instead of flowing into a column.

## Defining your own grid

For a one-off layout that isn't one of the presets, declare the columns inline with a `<!-- grid: … -->` comment. Cells then bind to those column names:

```markdown
<!-- grid: image 1fr | text 2fr -->

::: cell text
The text column is twice as wide as the image column.
:::

::: cell image
![Chart](/Users/me/talk/chart.png)
:::
```

The grid spec is a list of `name size` pairs separated by `|`. Sizes are any valid CSS grid track size (`1fr`, `2fr`, `200px`, `minmax(0, 1fr)`, …). You can also skip the names for plain equal or weighted columns:

- `<!-- grid: 2 -->` — two equal, unnamed columns.
- `<!-- grid: 1fr 2fr -->` — two unnamed columns, the second twice as wide.

A malformed spec is ignored (the slide falls back to its layout preset) and logs a warning.

## The `::: columns` shorthand

For a quick even split without naming columns, wrap the set in `::: columns` and put each part in its own `::: column`:

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

The `.columns` container lays out its `.column` children with `repeat(auto-fit, minmax(0, 1fr))` and a `2rem` gap. It spans the full width of whatever layout it sits on, so it works on any slide — use it when you just want N even columns and don't need to place content by name.

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
> Only **top-level** `::: notes` blocks are extracted. A `::: notes` block nested inside another fenced div (for example inside `::: columns` or a `::: cell`) is not picked up as notes — keep notes at the top level of the slide.

## Where to next

- [Content blocks](./04-content-blocks.md) — diagrams, charts, tables, math, and admonitions you can drop into any cell or column.
- [Themes](./05-themes.md) — the `.layout-<name>` selectors a custom theme can style.
- [PDF & image export](./06-pdf-export.md) — how speaker notes feed the PDF appendix.
