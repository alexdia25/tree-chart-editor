# Tree chart editor

A browser-based visual editor for MediaWiki's `{{Tree chart}}` and `{{familytree}}`
templates. Draw a family tree on a grid — lines, corners, tees, marriage
junctions, solid/dashed/dotted styles, coloured boxes — and get back wikitext
ready to paste into a wiki page. Also reads existing wikitext back in, so it
round-trips with pages that already use these templates.

No build step. Two files, no install, runs entirely in the browser.

## Try it

Open `tree-chart-editor-standalone.html` directly in a browser — nothing else
required.

## Use it as two files

`index.html` + `tree-chart-editor.jsx` do the same thing split apart, which is
easier to read and edit. Because browsers block a local page from `fetch()`-ing
a second local file, this pair needs to be served over http rather than opened
directly:

```bash
python -m http.server 8000
# or: npx serve .
```

Then open `http://localhost:8000`.

## What it does

- Draws lines, corners, tees, crossings and the marriage-junction tile on a
  grid, with independent solid/dashed/dotted styling for horizontal and
  vertical runs
- Places name boxes (three grid cells wide, matching the template's layout
  rules) with optional pastel or custom background colours
- Outputs either `{{Tree chart}}` or `{{familytree}}` syntax — the two
  templates use different tile character sets, decoded from
  `Module:Tree_chart/data` and the Familytree template source rather than
  guessed
- The wikitext panel is live and two-way: paste a chart in to load it, edit a
  row by hand and the grid follows, edit the grid and only the changed rows
  get rewritten (hand-formatting and surrounding wikitext elsewhere survive)
- Insert/delete rows and columns on the grid; drag boxes to reposition them

## Files

| File | Purpose |
|---|---|
| `tree-chart-editor.jsx` | The component. Single source of truth — edit this one. |
| `index.html` | Loads React/Babel from cdnjs, fetches the `.jsx` file, compiles it in-browser, mounts it. |
| `tree-chart-editor-standalone.html` | The same component inlined into one file, for double-click-and-go use with no second file to fetch. |

If you change `tree-chart-editor.jsx`, the standalone file needs regenerating
from it — it's a built artifact, not something to hand-edit.

## Limitations

- A handful of `{{Tree chart}}`'s documented "miscellaneous" tiles
  (`k3`, `T2`, `l3`, `l4`, `G2`, `b3`, `E`, `K`, `U`, `X`, `X2`) aren't
  decodable from the module's layout and are passed through unchanged on
  import rather than guessed at.
- `{{familytree}}` has no dotted-line tiles; the style picker disables that
  option when it's selected.
- This is a layout tool, not a pixel-accurate preview — final spacing in
  MediaWiki will differ slightly from the on-screen grid.
