# Getting started

MarkdownDeck turns a single Markdown file into a native-macOS presentation you can step through and export to PDF. You write plain Markdown, put `---` on its own line to split slides, and — optionally — open the file with a small YAML-style header to configure the whole deck. This page gets you installed, opens your first deck, and shows how to drive the deck view.

## Install

MarkdownDeck installs from the `slantedt/tap` Homebrew tap as the `mddeck` cask:

```bash
brew install --cask slantedt/tap/mddeck
```

This installs `MarkdownDeck.app` to `/Applications` and puts the `mddeck` command-line tool on your `PATH`. Confirm it worked:

```bash
mddeck --version
```

> [!NOTE]
> MarkdownDeck requires macOS 14 (Sonoma) or later.

## Your first deck

The fastest way to see MarkdownDeck in action is the bundled tutorial. The first time you launch the app with no recent deck and nothing passed on the command line, MarkdownDeck copies a tutorial deck to `~/Documents/MarkdownDeck Tutorial.md` and opens it. You can edit that file freely — MarkdownDeck never overwrites your copy.

You can reopen the tutorial at any time from **Help → "MarkdownDeck Tutorial"**. If you have already edited it, that menu item opens your edited copy rather than replacing it.

To open a deck of your own, pass it to the CLI:

```bash
mddeck path/to/deck.md
```

> [!TIP]
> A deck is just Markdown. Start a new file, drop a `---` on its own line wherever you want a slide break, and open it with `mddeck` to watch it render.

## Stepping through a deck

Once a deck is open, the deck view responds to these keys:

| Key | Action |
|---|---|
| `→` · `Space` · `Page Down` | Next slide |
| `←` · `Page Up` | Previous slide |
| `⌘→` · `⌘←` | Jump 5 slides forward / back |
| `Home` · `End` | First / last slide |
| `F` | Enter fullscreen |
| `Esc` | Exit fullscreen |
| `⌘⇧P` | Export to PDF |

## Where to next

- [Authoring a deck](./02-authoring.md) — front-matter keys, splitting slides, and writing slide content.
- [CLI reference](./08-cli-reference.md) — every `mddeck` verb, its flags, and exit codes.
