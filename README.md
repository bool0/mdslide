# MD Slide

A lightweight, **zero-dependency** Markdown → slides tool that runs entirely in your browser. One HTML file, no installation, no build step, no network required. Works on Windows and macOS.

**To use it: download [`mdslide.html`](mdslide.html) and double-click it.** That's the whole setup.

## Features

- **Live preview** — edit Markdown on the left, see the 16:9 slide on the right instantly; the preview follows your cursor
- **5 built-in themes** — Clean, Dark, Academic, Gradient, Paper
- **PDF export** — one click, each slide becomes one page (via the browser's print engine)
- **Project folders** — open a local folder, browse its files in a sidebar, save and load `.md` files in place (Chrome/Edge)
- **Local images** — insert images from disk; they're copied into your project folder so references survive reloads
- **Mind maps** — draw one from an indented list, rendered as SVG
- **Math** — `$inline$` and `$$display$$` TeX subset: fractions, roots, sub/superscripts, Greek letters, big operators, matrices
- **Citations & BibTeX** — cite with `[@key]`, load `.bib` files or embed one in the document, auto-generated reference list with overflow pagination
- **Multi-column layouts** — split any slide into columns
- **Nested lists, tables, code blocks with line numbers**, blockquotes, links
- **Autosave** — content persists in `localStorage`; keyboard navigation with `←` `→`, save with `Ctrl/Cmd+S`

## Syntax

### Slides

Separate slides with `---` on its own line:

```markdown
# My Deck

A slide with only a big heading becomes a centered title slide.

---

## Second Slide

- Bullet points
  - Nested: indent by two spaces
    - Third level
```

### Columns

Split a slide with `|||` on its own line. Headings above the first `|||` stay full-width:

```markdown
## Text Left, Image Right

Some explanation text.

|||

![figure](figure.png)
```

Use `||| 2` to make the next column twice as wide. More `|||` markers give you three or more columns.

> `|||` is custom syntax of this tool — standard Markdown has no column syntax, so other renderers will show it as plain text.

### Mind maps

A fenced `mindmap` block turns an indented list into a colored mind map:

````markdown
```mindmap
- Root
  - Branch A
    - Leaf 1
    - Leaf 2
  - Branch B
```
````

### Math

Standard TeX-style delimiters — `$...$` inline, `$$...$$` display:

```markdown
Inline: $E = mc^2$, $\alpha + \beta \le \gamma$

$$ \mathrm{MSE} = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2 $$

$$ A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} $$
```

Supported subset: `^` `_` sub/superscripts, `\frac`, `\sqrt`, Greek letters, ~90 common symbols (`\le`, `\infty`, `\in`, `\to`, `\partial`, …), big operators (`\sum` `\prod` `\int`), functions (`\sin` `\log` `\lim`, …), accents (`\bar` `\hat` `\vec`), `\mathbf` `\mathrm` `\text`, and matrix environments (`matrix`, `pmatrix`, `bmatrix`, `vmatrix`, `Vmatrix`, `cases`). Rendered with pure HTML/CSS — no external libraries. `\begin{align}` and other multi-line alignment environments are not supported.

Dollar amounts like `$5 and $10` are left alone (math requires non-space characters directly inside the delimiters).

### Citations

Cite with Pandoc-style keys, and put `[references]` on its own line where the bibliography should appear:

```markdown
Transformers changed everything [@vaswani2017], and pre-training
made them practical [@vaswani2017; @devlin2019].

---

## References

[references]
```

Two citation styles, switchable in the toolbar:

- **Numeric** — `[1]`, `[1, 2]`, references numbered in citation order
- **Author–Year** — `(Vaswani et al., 2017)`, references sorted alphabetically

Reference lists that don't fit on one slide **paginate automatically** across as many slides as needed, with continuous numbering.

Load BibTeX entries three ways:

1. **Project folder** — every `.bib` file in the folder is parsed automatically
2. **Import button** — also accepts `.bib` files
3. **Embedded** — put entries in a fenced `bibtex` block inside the document itself (renders as nothing, so a single `.md` file can carry its own bibliography):

````markdown
```bibtex
@inproceedings{vaswani2017,
  author    = {Ashish Vaswani and Noam Shazeer},
  title     = {Attention Is All You Need},
  booktitle = {NeurIPS},
  year      = {2017}
}
```
````

Unknown keys render as red `[?key]` so typos are easy to spot.

## PDF export

Click **⬇ Download PDF**, then choose *Save as PDF* in the print dialog. Each slide is exactly one 960×540 px page with backgrounds preserved.

Tips:

- Chrome or Edge give the best results (they honor the custom page size)
- If you see a URL/date in the margins, uncheck *Headers and footers* in the print dialog
- `Ctrl/Cmd+P` works too

## Project folders, saving, and images

Click **📁 Project** and pick a folder (Chrome/Edge; uses the [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API)):

- The sidebar lists the folder tree — click a 📝 `.md` to open it, a 🖼 image to insert a reference, a 📚 `.bib` to (re)load it
- **💾 Save** (`Ctrl/Cmd+S`) writes back to the opened file; with no file open it creates one in the folder
- **🖼 Image** copies the picked image into the project folder and inserts `![name](name)` at the cursor
- Reference images in Markdown by filename or relative path — they resolve against the project folder

On Safari/Firefox the folder features are unavailable; **Import** (file picker) and **Save** (download) still work.

## Browser support

| | Chrome / Edge | Safari / Firefox |
|---|---|---|
| Editing, preview, themes, math, mind maps, citations | ✅ | ✅ |
| PDF page size honored | ✅ | ⚠️ falls back to paper size |
| Project folder / save in place | ✅ | ❌ (import/download fallback) |

## Development

Everything lives in a single `mdslide.html` — vanilla JS, no build. Open it, edit it, refresh. The file contains, in order: theme CSS, print CSS, a small TeX renderer, a Markdown parser, a BibTeX parser, the mind-map layout engine, and the app wiring.

## License

[Apache 2.0](LICENSE)
