# Authoring a deck

A MarkdownDeck deck is a single Markdown file, where an optional header block at the top configures the whole deck and `---` on its own line splits the file into slides. This page covers that header (the "front-matter"), how slides are split, the content you can put on a slide, and the one gotcha around images.

## Front-matter

Front-matter is an optional configuration block at the very top of the file. It is fenced by a line that is only `---` above and below, with one `key: value` per line in between:

```markdown
---
title: Quarterly review
author: Jane Doe
date: 2026-06-09
aspect: "16:9"
theme: keynote-noir
page_numbers: true
header: ""
footer: Confidential
title_page: true
notes_appendix: true
overflow_badges: false
---

# First slide
```

A few parsing rules worth knowing:

- Values may be single- or double-quoted. Quotes are optional and are stripped only when they match (an opening `"` paired with a closing `"`).
- A line with no `:` is ignored.
- If the opening `---` has no matching closing `---`, the entire block is ignored and you get the warning `front-matter missing closing ---; ignored`.

### Keys

| Key | Type | Default | Notes |
|---|---|---|---|
| `title` | string | `""` | used by `title_page` |
| `author` | string | `""` | used by `title_page` |
| `date` | string | `""` | free-form; you choose the format |
| `aspect` | `"16:9"` or `"4:3"` | `16:9` | unknown value → falls back to 16:9 with a warning. 16:9 = 1280×720, 4:3 = 1280×960 (px @96dpi) |
| `theme` | string | `default` | a built-in name or a custom theme name — see [Themes](./05-themes.md) |
| `page_numbers` | bool | `false` | |
| `header` | string | `""` | running header shown on every slide when non-empty |
| `footer` | string | `""` | running footer shown on every slide when non-empty |
| `title_page` | bool | `false` | synthesizes a `cover` slide from title/author/date |
| `notes_appendix` | bool | `true` | append speaker notes as a PDF section — the only boolean defaulting to true |
| `overflow_badges` | bool | `false` | also show the overflow badge in the PDF |

> [!NOTE]
> Booleans accept `true`, `yes`, or `1` (case-insensitive); anything else is treated as false. So `page_numbers: yes` and `page_numbers: 1` both turn page numbers on.

Front-matter is entirely optional. Omit the block and every key takes its default — a 16:9 deck on the `default` theme with the notes appendix on.

## Splitting slides

A line that, after trimming whitespace, is only `---` starts a new slide. Everything above the first split is slide one; everything after the last split is the final slide.

```markdown
# Slide one

Some content.

---

# Slide two

More content.
```

Two `---` rules keep the split predictable:

- A `---` inside a fenced code block (```` ``` ```` or `~~~`) does **not** split — it is part of your code.
- A `---` inside a `::: … :::` fenced div (see [Slide layouts](./03-layouts.md)) does **not** split either.

Leading or trailing `---` lines do not create empty slides, so a stray separator at the start or end of the file is harmless.

## Slide content

Slide bodies are standard GitHub-Flavored Markdown: headings, paragraphs, `**bold**` and `*italic*`, ordered and unordered lists, links, inline `code` and fenced code blocks, and images.

```markdown
# Roadmap

We are shipping three things this quarter:

- **Search** — full-text, instant
- **Themes** — six built-ins plus your own CSS
- **Export** — PDF and per-slide PNGs

See the [release notes](https://example.com/notes) for details.
```

For richer blocks — diagrams, charts, CSV/TSV tables, math, and admonitions — see [Content blocks](./04-content-blocks.md). To control how a slide is arranged, see [Slide layouts](./03-layouts.md).

## Images

Images use the standard Markdown syntax, `![alt](URL)`.

> [!WARNING]
> On the current build, a bare relative path like `![](pic.png)` will **not** resolve to a file sitting next to your deck. The deck HTML is loaded with a base URL pointing at the app bundle, not your deck's folder, so relative paths are resolved against the bundle. Use an absolute path or a full URL instead:
>
> ```markdown
> ![Architecture](/Users/me/talk/pic.png)
> ![Logo](https://example.com/logo.png)
> ```

This is purely about how the path is resolved — `https://…` URLs and absolute filesystem paths both work as expected.

## Where to next

- [Slide layouts](./03-layouts.md) — the eight layouts, columns, and speaker notes
- [Content blocks](./04-content-blocks.md) — diagrams, charts, tables, math, admonitions
- [Themes](./05-themes.md) — built-in themes and custom CSS
- [PDF & image export](./06-pdf-export.md) — what the front-matter keys do at export time
