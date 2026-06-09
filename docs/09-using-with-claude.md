# Using with Claude

MarkdownDeck ships a Claude skill so an AI assistant already knows the deck format, the `mddeck` CLI, and the visual editor. This page shows how to extract that skill into your Claude setup and what it unlocks once it's there.

## The bundled skill

MarkdownDeck bundles an `mddeck` Claude skill inside the app. It teaches Claude the slide markdown format (front-matter, `---` slide splits, the eight layouts), the content blocks (Mermaid/PlantUML, Vega-Lite charts, CSV/TSV tables, KaTeX math, admonitions), the `mddeck` CLI verbs, and the macOS editor's shortcuts. With it installed, Claude can draft and edit decks in the exact syntax MarkdownDeck renders — no guessing at conventions.

The skill lives in the app bundle; you copy it out into your own skills directory with one command.

## Installing the skill

Extract the bundled skill into your Claude skills directory:

```bash
mddeck skill extract
```

That drops it into `~/.claude/skills/mddeck/`. Restart Claude (or reload skills) and it becomes available.

You can change where it lands and how it behaves:

| Argument / flag | Effect |
|---|---|
| `<dest>` | Extract to this directory instead of the default (tilde-expanded). |
| `--force` | Overwrite an existing destination. |
| `--quiet` | Suppress the "Extracted mddeck skill to …" success line. |

```bash
mddeck skill extract ~/my-skills/mddeck   # custom location
mddeck skill extract --force              # overwrite an existing copy
mddeck skill extract --quiet              # no success line (for scripts)
```

> [!NOTE]
> If the destination already exists, `mddeck skill extract` refuses to clobber it and exits `1`. Pass `--force` to overwrite. See [`./08-cli-reference.md`](./08-cli-reference.md) for the full exit-code table.

## Example prompts

Once the skill is installed, you can drive deck authoring in plain language. A few prompts that work well:

- "Turn these notes into a MarkdownDeck deck."
- "Add a two-column slide with an image on the right."
- "Export this deck to PDF with `mddeck export`."

Claude will produce (or edit) a Markdown file in MarkdownDeck's syntax — front-matter at the top, `---` between slides, `<!-- layout: … -->` directives, and the right fenced blocks for diagrams, charts, tables, and math.

> [!TIP]
> Pair the prompts with [`./02-authoring.md`](./02-authoring.md) when you want to verify a key name or default by hand — the skill follows the same front-matter contract documented there.

## See also

- [`./02-authoring.md`](./02-authoring.md) — the deck format the skill writes for you.
- [`./08-cli-reference.md`](./08-cli-reference.md) — every `mddeck` verb, flag, and exit code, including `skill extract`.

Authoring a Sigla-style extension (a `sigla.json` manifest with custom fence-block renderers) is out of scope here. MarkdownDeck shares Sigla's extension model, so for that, use Sigla's `plugin-dev` skill.
