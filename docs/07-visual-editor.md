# The visual editor

MarkdownDeck is a Markdown editor at heart, but it also gives you mouse-and-keyboard tools that edit your deck *for* you while keeping the file canonical and hand-editable. This page covers the command palette, the Slide Inspector (layout picker + add-content grid), the read-only grid overlay, and finding text — and the exact keyboard shortcuts for each.

Every command here is a plain, deterministic edit to your Markdown: switching a layout rewrites the `<!-- layout: … -->` line exactly as you would by hand, and adding a block inserts ordinary fenced Markdown. Nothing is hidden in a binary format, and everything is undoable with `⌘Z`.

## Panels & shortcuts

| Shortcut | Action |
|---|---|
| `⌘K` | Command palette — fuzzy search over every editor action |
| `⌘⌥I` | Slide Inspector (layout picker, add-content grid, grid overlay) |
| `⌘⌥S` | Source panel |
| `⌘⇧T` | Table of Contents |
| `⌘⌥D` | Layout Diagnostics |
| `⌘⇧C` | Wrap the selection in columns |
| `⌘⌫` | Delete the current slide (or the selected block) |

The command palette (`⌘K`) is the fastest way in: type a few letters and it fuzzy-ranks every available action — switching a layout, adding a block, toggling a panel, wrapping in columns — then run the highlighted one with `⏎`. Use `↑`/`↓` to move and `⎋` to dismiss.

> [!TIP]
> The same actions live in the menus and the Slide Inspector, so you never have to remember a shortcut. The palette just gets you there without leaving the keyboard.

## The Slide Inspector

Open the inspector with `⌘⌥I`. Its header reads **"Slide N of M"** for the slide you're currently viewing, and the pane reflects whatever slide you navigate to.

The top of the inspector is the **layout picker**: eight mini-diagram buttons, one per layout (`default`, `cover`, `section`, `two-cols`, `image-left`, `image-right`, `full-image`, `end`), each drawing a small schematic of that layout's grid. The current slide's layout button is highlighted.

- **Hovering** a layout button previews that layout live on the active slide — without touching your file. Move the pointer away and the slide reverts.
- **Clicking** a layout button rewrites the slide's `<!-- layout: … -->` directive (inserting one if the slide had none, always at the first non-blank line) and re-renders. The change is undoable with `⌘Z`.
- **Clicking the layout the slide already has is a no-op** — the source is left untouched.

See [`./03-layouts.md`](./03-layouts.md) for what each of the eight layouts does.

## Adding content

Below the layout picker, the inspector has an **add-content grid**: ten labelled buttons that each insert a block at the end of the current slide. The exact same ten items are available from the command palette as "Add …" commands, so you can insert them by keyboard too.

| Item | Inserts |
|---|---|
| Heading | `## Heading` |
| Text | `Body text.` |
| Bullets | `- Item one` / `- Item two` / `- Item three` |
| Image | `![alt](path/to/image.png)` |
| Columns | a `::: columns … :::` scaffold |
| Code | a ` ```swift ` block |
| Table | a 2×2 GFM table |
| Mermaid | a ` ```mermaid ` flowchart |
| Chart | a ` ```vega-lite ` spec |
| Math | `$$`<br>`E = mc^2`<br>`$$` |

Inserted blocks are plain Markdown — the same thing you'd type by hand — so they stay editable in the source and round-trip cleanly. See [`./04-content-blocks.md`](./04-content-blocks.md) for what each block type renders to.

The inserted `::: columns` scaffold is for a quick *even* split. To place content into the **named** columns of a multi-column layout — `left`/`right` on `two-cols`, or `image`/`text` on `image-left`/`image-right` — wrap it in a `::: cell <name>` block instead. See [`./03-layouts.md`](./03-layouts.md#placing-content-into-columns-with-cells).

> [!NOTE]
> The inserted Image snippet uses a placeholder path. Replace it with an absolute path or an `https://` URL — bare deck-relative paths like `pic.png` do not resolve on `main`. See the image note in [`./02-authoring.md`](./02-authoring.md).

## The grid overlay

The bottom of the inspector has a **Grid overlay** checkbox. Turn it on and the active slide is overlaid with its CSS-grid lines, Firefox-DevTools style. Four sub-toggles control what's drawn:

- **Line numbers** — number each grid line.
- **Track sizes** — show each track's resolved pixel size.
- **Gaps** — shade the gaps between tracks.
- **Overflow** — for a slide whose content overflows, draw a box showing the pre-shrink content extent against the slide frame, labelled with the active auto-shrink scale.

The overlay is strictly **read-only**: it never changes your Markdown, never affects the slide's own layout or auto-shrink scaling, and is never rendered in PDF or PNG exports. It stays registered to the rendered grid even as you resize the window, and re-applies itself after an edit re-renders the deck.

> [!NOTE]
> With the overlay on, clicking an empty area of the active slide opens the command palette pre-filtered to the "Add …" commands — a quick way to drop a block exactly where the layout has room. (This gesture is ignored on a synthesized title page.)

## Finding text

| Shortcut | Action |
|---|---|
| `⌘F` | Open the find bar |
| `⌘G` | Find next |
| `⌘⇧G` | Find previous |

## Where to next

- [`./03-layouts.md`](./03-layouts.md) — what the eight layouts do.
- [`./04-content-blocks.md`](./04-content-blocks.md) — the blocks the add-content grid inserts.
- [`./08-cli-reference.md`](./08-cli-reference.md) — drive the same deck from the `mddeck` CLI.
