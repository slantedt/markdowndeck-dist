# CLI reference

The `mddeck` command-line tool opens decks in MarkdownDeck and renders them headlessly to PDF or PNG — no GUI session required, which makes it the piece you wire into scripts and CI. This page documents every verb, its flags, the `--json` payloads, and the exit codes.

## Synopsis

```
mddeck [deck.md ...]
mddeck export <input> [-o <out.pdf>] [--force] [--json]
mddeck snapshot <input> [-o <dir>] [--scale <n>] [--force] [--json]
mddeck skill extract [<dest>] [--force] [--quiet]
mddeck --help | --version
```

## `mddeck` — open decks (default)

Run `mddeck` with zero or more file paths to launch or activate MarkdownDeck and open those decks. Paths are tilde- and glob-expanded by your shell, then resolved before opening:

```bash
mddeck                       # just launch / bring the app forward
mddeck talk.md               # open one deck
mddeck ~/talks/*.md          # open every deck the glob matches
```

Exits `0` on success, or `1` if a file isn't found or the app fails to launch.

## `mddeck export` — render a PDF

Renders the deck to a PDF using the same per-slide exporter the app uses.

| Flag | Default | Meaning |
|---|---|---|
| `-o`, `--output <path>` | `<basename>.pdf` in the current directory | where to write the PDF |
| `--force` | off | overwrite an existing output file |
| `--json` | off | print a machine-readable result on stdout |

```bash
mddeck export talk.md                      # → ./talk.pdf
mddeck export talk.md -o out/deck.pdf --force
mddeck export talk.md --json
```

> [!NOTE]
> Front-matter keys still apply to headless exports: `notes_appendix` (on by default), `overflow_badges`, `page_numbers`, `header`, and `footer`. See [./06-pdf-export.md](./06-pdf-export.md) for what ends up in the PDF.

With `--json`, the result is printed to stdout (schema `mddeck-export/v1`). `scaled_slides` lists the indices of slides that were auto-shrunk to fit, which makes it useful as a CI overflow gate. The `warnings` array carries non-fatal authoring diagnostics — for example a `::: cell` whose name matches no column on its slide (it auto-flows into the next free column instead), or a malformed `<!-- grid: … -->` spec (ignored, falling back to the layout preset). See [./03-layouts.md](./03-layouts.md) for the named-column and grid model these warnings come from:

```json
{
  "schema": "mddeck-export/v1",
  "input": "talk.md",
  "input_sha256": "…",
  "primary_output": "/abs/path/talk.pdf",
  "slides": 12,
  "scaled_slides": [4, 7],
  "warnings": [],
  "elapsed_ms": 813
}
```

## `mddeck snapshot` — per-slide PNGs

Renders each slide to its own PNG.

| Flag | Default | Meaning |
|---|---|---|
| `-o`, `--output-dir <dir>` | current directory | directory to write PNGs into |
| `--scale <n>` | `1.0` | multiplier on top of the captured backing scale |
| `--force` | off | overwrite existing PNGs |
| `--json` | off | print a machine-readable result on stdout |

Files are named `<basename>-slide-<n>.png`, where `n` is 1-based:

```bash
mddeck snapshot talk.md -o frames/   # → frames/talk-slide-1.png, talk-slide-2.png, …
```

> [!TIP]
> `--scale` defaults to `1.0` because the offscreen WebView already captures at the display's backing scale — roughly 2× on a Retina display — so `1.0` already yields a crisp ~2× PNG. Raise it only when you need even larger output.

With `--json`, the result is printed to stdout (schema `mddeck-snapshot/v1`):

```json
{
  "schema": "mddeck-snapshot/v1",
  "input": "talk.md",
  "output_dir": "/abs/path/frames",
  "slides": 12,
  "files": ["/abs/path/frames/talk-slide-1.png", "…"],
  "scale": 1.0,
  "elapsed_ms": 1042
}
```

## `mddeck skill extract` — install the Claude skill

Copies the bundled `mddeck` Claude skill out of the app and into your skills directory.

| Flag | Default | Meaning |
|---|---|---|
| `<dest>` | `~/.claude/skills/mddeck` | where to install the skill |
| `--force` | off | overwrite an existing destination |
| `--quiet` | off | suppress the success line |

```bash
mddeck skill extract                 # → ~/.claude/skills/mddeck
mddeck skill extract ./skills --force
```

Re-running when the destination already exists exits `1` unless you pass `--force`. See [./09-using-with-claude.md](./09-using-with-claude.md) for what the skill unlocks.

## Global flags

`--help` / `-h` and `--version` / `-v` / `-V` are recognized anywhere on the command line and exit `0`.

```bash
mddeck --help
mddeck --version
```

## Exit codes

| Code | Class | When |
|---|---|---|
| `0` | OK | command succeeded |
| `1` | User error | bad arguments or usage; output exists without `--force`; `skill` destination exists without `--force`; a file passed to open isn't found |
| `2` | Render error | the deck has no slides; PDF export or snapshot rendering failed |
| `3` | I/O error | can't read the input; `MarkdownDeck.app` (and its bundled resources) not found; can't write the output; the bundled skill is missing |

> [!NOTE]
> Headless `export` and `snapshot` borrow the app's resource tree (deck template, themes, KaTeX), so `MarkdownDeck.app` must be installed. If it lives somewhere unusual, point the CLI at it with `MDDECK_APP_PATH=/path/to/MarkdownDeck.app`.

## Where to next

- [PDF & image export](./06-pdf-export.md) — what the front-matter keys do at export time.
- [Using with Claude](./09-using-with-claude.md) — what `mddeck skill extract` installs.
