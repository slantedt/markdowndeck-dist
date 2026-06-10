# Themes

A theme controls how your deck looks — colors, type, spacing, and the styling of every block. MarkdownDeck ships six built-in themes you select with one front-matter line, and it lets you drop in your own CSS theme that layers over the same structural base. This page covers both, the CSS contract a custom theme can override, and how to switch themes from the app without touching your file.

## Built-in themes

Set a theme with the `theme:` key in front-matter:

```markdown
---
theme: keynote-noir
---

# My deck
```

There are six built-in themes:

| `theme:` value | Look |
|---|---|
| `default` | Clean light theme — white background, blue accent |
| `dark` | Dark background, light text |
| `editorial-serif` | Serif typography, print-editorial feel |
| `keynote-noir` | Near-black slides with an amber accent (Keynote-style) |
| `warm-minimal` | Sand-and-clay palette with a terracotta accent |
| `bold-geometric` | High-contrast cobalt two-tone |

An unknown `theme:` value is treated as a [custom CSS theme](#custom-css-themes) (see below), not an error — so check the spelling if a built-in theme isn't applying.

## Custom CSS themes

To brand a deck without editing the app, drop a `<name>.css` file in:

```
~/.config/markdowndeck/themes/
```

> [!NOTE]
> The themes directory is XDG-aware. If `XDG_CONFIG_HOME` is set, MarkdownDeck looks in `$XDG_CONFIG_HOME/markdowndeck/themes` instead of `~/.config/markdowndeck/themes`.

Then reference the file's stem (its name without `.css`) in front-matter:

```markdown
---
theme: nord
---
```

A custom theme **layers over** the bundled structural CSS — it is loaded on top of the default theme (served internally via `mddeck://theme/<name>.css`), so you only write the overrides you care about. If the named file is missing, the deck still renders using the default theme rather than failing.

A minimal `~/.config/markdowndeck/themes/nord.css` that just retints the deck:

```css
:root {
  --bg: #2e3440;
  --fg: #eceff4;
  --accent: #88c0d0;
}
```

That's a complete custom theme — three variables. Everything else (layout, spacing, code, tables) inherits from the structural base.

## CSS variables

A custom theme overrides these CSS variables, defined on `:root` in the base theme:

| Variable | Default | Purpose |
|---|---|---|
| `--bg` | `#ffffff` | Slide background |
| `--fg` | `#1d1d1f` | Foreground / body text |
| `--muted` | `#6b6b70` | Muted / secondary text |
| `--accent` | `#0066cc` | Links and accents |
| `--code-bg` | `#f5f5f7` | Code background |
| `--code-fg` | `#1d1d1f` | Code text |
| `--chart-categorical` | Okabe-Ito palette | Comma-separated chart series colors |
| `--slide-pad` | `4rem` | Slide padding |
| `--base-size` | `28px` | Root font size |

Two variables are read-only — they're set from the deck's aspect ratio and shouldn't be overridden:

| Variable | Value | Notes |
|---|---|---|
| `--slide-w` | `1280px` | Slide width |
| `--slide-h` | `720px` (16:9) / `960px` (4:3) | Slide height; switches with `aspect:` |

## Selectors you can target

Beyond the variables, you can style any element directly. The `<body>` carries classes you can scope to:

- `body.theme-<name>` — the active theme (e.g. `body.theme-dark`)
- `body.mode-screen` / `body.mode-pdf` — on-screen vs. PDF rendering
- `body.aspect-4-3` — 4:3 decks (16:9 is the default)

Per-slide and content selectors:

- `.slide`, `.slide.active`, `.slide.overflow`
- `.slide.layout-<layout>` — e.g. `.slide.layout-cover`, `.slide.layout-two-cols`
- `.slide .content`, `.slide .columns`, `.slide .column`, `.slide .cell` — `.cell` is the element a `::: cell <name>` block renders to (see [Slide layouts](./03-layouts.md))
- Headings, `p`, `a`, `li`, `ul`, `ol`
- `code`, `pre`, `pre code`
- `table`, `th`, `td`, `thead th`
- `img`, `.mermaid`, `svg`
- `blockquote`
- `.admonition` plus `.admonition-note`, `.admonition-tip`, `.admonition-important`, `.admonition-warning`, `.admonition-caution`, and `.admonition-title`

For example, to give code blocks a tighter look in a custom theme:

```css
.slide pre { border-radius: 12px; font-size: 0.65em; }
.slide.layout-cover h1 { letter-spacing: -0.02em; }
```

> [!TIP]
> Admonition styling comes from these selectors. See [Content blocks](./04-content-blocks.md) for the admonition syntax (`> [!NOTE]`, `> [!TIP]`, and friends).

## Switching themes in the app

You don't have to edit your file to try a theme. **View → Deck Theme** lists:

- **From Document** — honor the deck's own front-matter `theme:`
- the six built-in themes
- any custom themes found in your themes directory (freshly dropped files appear without relaunching)

Choosing a theme applies it as a render-time override across the deck.

> [!NOTE]
> The Deck Theme override is **not** written into your Markdown file — it's a preference (stored under the `DeckThemeOverride` key). The same override is honored by `mddeck export`, so a PDF you export reflects the theme you picked in the menu. Pick **From Document** to clear the override and go back to the deck's front-matter theme.

## See also

- [Authoring a deck](./02-authoring.md) — where the `theme:` and `aspect:` front-matter keys live.
- [Content blocks](./04-content-blocks.md) — the blocks (admonitions, tables, charts, code) your theme styles.
- [PDF & image export](./06-pdf-export.md) — how the Deck Theme override carries into exported PDFs.
