# editable-html-slides

> A zero-dependency, in-browser **WYSIWYG editor** you can bolt onto any HTML
> slide deck — so the deck stays **editable after it's generated**.

AI coding agents are great at *generating* beautiful HTML decks, but the output
is read-only to a normal user — to change a title or swap an image they'd have to
open the source. This skill closes that gap with two small files
(`editor.js` + `editor.css`) that layer a visual editor on top of an existing
deck, **without touching its design** and **without a build step, Node, or npm**.

It pairs especially well with [html-ppt](https://github.com/lewislulu/html-ppt-skill)
/ reveal-style decks — it preserves their presenter view and speaker notes — but
works on any deck using the `.slide` / `.is-active` convention.

## Features

- ✏️ **Inline text editing** — click any heading/paragraph/list item and type
- 🖼 **Add images** — local file (embedded as data URL) or paste with ⌘/Ctrl+V
- 🔤 **Add text boxes** — free-floating, double-click to edit
- ✋ **Drag / resize / delete** added objects
- ↶ **Undo / redo** — ⌘/Ctrl+Z, ⌘/Ctrl+Shift+Z (80-step snapshot history)
- ▦ **Reorder slides** — draggable thumbnail filmstrip
- 💾 **Autosave** to `localStorage` + ⬇ **export** a clean standalone HTML
- 🎤 Non-edit mode is unchanged — still presentable (S / arrows / themes, etc.)

Toggle with the **✎ button** or the **E** key. **Esc** / ✓ to exit.

## Install as a Claude skill

```bash
npx skills add rossyao2022/editable-html-slides -g
```

or clone into your skills directory:

```bash
git clone https://github.com/rossyao2022/editable-html-slides \
  ~/.claude/skills/editable-html-slides
```

## Use it on a deck

```bash
python3 scripts/inject.py /abs/path/to/your-deck/index.html
# then open index.html and press E
```

The injector copies `editor.css` + `editor.js` next to your deck and wires two
tags in (idempotently). Prefer to do it by hand? Add:

```html
<link rel="stylesheet" href="editor.css">      <!-- in <head> -->
<script src="editor.js"></script>               <!-- before </body>, AFTER the deck's runtime -->
```

## Try the demo

Open [`examples/demo.html`](examples/demo.html) — a tiny self-contained deck (with
a 30-line minimal runtime) — and press **E**. No other dependencies.

## How it works

See [`references/runtime-notes.md`](references/runtime-notes.md) for the design:
the **slot-fixed, content-swap** reorder model (so it never disturbs a presenter
runtime's cached page order), the snapshot-based undo stack, the `.overview`
double-count gotcha, and how to retarget the selectors for a non-standard deck.

## Requirements

The deck's slides should use class `.slide`, with the visible one marked
`.is-active`, ideally inside a `.deck` container. Speaker notes in
`<aside class="notes">` are preserved and never turned into editable on-slide
text. No Node, npm, build, or network required.

## License

MIT © rossyao2022
