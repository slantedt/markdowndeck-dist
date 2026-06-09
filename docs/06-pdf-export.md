# PDF & image export

MarkdownDeck turns any deck into a polished PDF — either interactively, from a preview window inside the app, or headlessly from the command line. It can also write one PNG per slide. This page covers all three, plus the front-matter keys that change what lands in the PDF.

## Exporting from the app

Press `⌘⇧P` (**File → Export as PDF…**) to open the PDF preview window. It renders the whole deck to a live PDF, with a thumbnail rail down the side; pages whose content was auto-scaled to fit are flagged with an orange warning marker next to their thumbnail.

The window's toolbar carries three chrome toggles and two buttons:

- **Page numbers**, **Footer**, **Header** — checkboxes that re-render the preview as you flip them. Each toggle overrides your deck's front-matter for this export (see [What ends up in the PDF](#what-ends-up-in-the-pdf) below). Your choices are remembered between exports.
- **Export…** writes the previewed PDF to a location you pick.
- **Print…** sends the previewed PDF to the system print dialog.

`⌘P` (**File → Print…**) opens the same preview window and primes the **Print…** button, so printing goes through the identical rendering path as export — what you print is exactly what you'd export.

> [!NOTE]
> The preview tracks the deck file live. Edit and save the source while the preview is open and it re-renders automatically. It also follows the **View → Deck Theme** override, so the PDF matches what the app is showing on screen.

## Headless PDF export

`mddeck export` renders a deck to a PDF without launching the UI — ideal for scripts and CI.

```bash
mddeck export <input> [-o <out.pdf>] [--force] [--json]
```

- `-o` / `--output` — output path. Defaults to `<basename>.pdf` in the current directory.
- `--force` — overwrite an existing output file (otherwise the command refuses and exits non-zero).
- `--json` — print a machine-readable result instead of the human summary.

```bash
# Writes ./talk.pdf
mddeck export talk.md

# Explicit destination, overwriting if it exists
mddeck export talk.md -o ~/Desktop/talk.pdf --force
```

For the `--json` payload shape and the full exit-code table, see [CLI reference](./08-cli-reference.md).

## Per-slide PNG snapshots

`mddeck snapshot` renders each slide to its own PNG — handy for thumbnails, social cards, or dropping a single slide into another document.

```bash
mddeck snapshot <input> [-o <dir>] [--scale <n>] [--force] [--json]
```

- `-o` / `--output-dir` — directory for the PNGs. Defaults to the current directory.
- `--scale` — pixel-density multiplier; default `1.0`.
- `--force` — overwrite existing PNGs.
- `--json` — print a machine-readable result.

Files are named `<basename>-slide-<n>.png`, where `n` is **1-based**. A deck named `talk.md` produces `talk-slide-1.png`, `talk-slide-2.png`, and so on. If the deck synthesizes a `title_page` cover slide, it counts as slide 1.

```bash
# Writes talk-slide-1.png … talk-slide-N.png into ./out/
mddeck snapshot talk.md -o out/
```

> [!TIP]
> You usually don't need `--scale`. The offscreen renderer already captures at the display's backing scale (roughly 2× on a Retina Mac), so the default `1.0` already yields a crisp, high-resolution PNG. Raise `--scale` only when you need an even larger image.

## What ends up in the PDF

A handful of front-matter keys shape the exported PDF. (See [Authoring a deck](./02-authoring.md#front-matter) for the full key reference.)

| Key | Default | Effect on the PDF |
|---|---|---|
| `notes_appendix` | **`true`** | Appends speaker notes as a themed section after the slides |
| `overflow_badges` | `false` | When `true`, the `⚠ overflow` badge appears in the PDF too (it's hidden by default) |
| `page_numbers` | `false` | Prints a page number on each slide |
| `header` | `""` | Prints a running header on every slide when non-empty |
| `footer` | `""` | Prints a running footer on every slide when non-empty |

**Speaker notes appendix.** When `notes_appendix` is on (the default), any `::: notes … :::` blocks are collected and appended to the PDF as a separate, themed section. The appendix is only added if at least one slide actually has notes — a deck with no notes produces no appendix. See [Slide layouts](./03-layouts.md#speaker-notes) for how to write notes.

**The chrome toggles override front-matter, per export.** The **Page numbers**, **Footer**, and **Header** checkboxes in the preview window win over whatever the front-matter says — both directions. Turning a toggle **off** suppresses that chrome even if front-matter requested it; turning it **on** lets the front-matter value through.

> [!NOTE]
> For footer and header, the toggle only controls *visibility* — it doesn't supply text. Turning **Header** on while the front-matter `header:` is empty shows nothing, because there's no text to display.

## Where to next

- [CLI reference](./08-cli-reference.md) — every `mddeck` verb, the `--json` payloads, and exit codes.
- [Slide layouts](./03-layouts.md) — columns and the `::: notes … :::` blocks that feed the appendix.
