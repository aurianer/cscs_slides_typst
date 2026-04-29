# CSCS Slides — Typst

A Typst port of the CSCS Beamer presentation theme.

10-slide template covering: title, table of contents, chapter divider,
citations, color blocks, formulas, code listings, thank-you, and references.

## Requirements

- [Typst](https://typst.app/) ≥ 0.13 (tested with 0.14.2)
- Helvetica-clone font for body text; fallback chain
  `TeX Gyre Heros → Nimbus Sans → Liberation Sans`.
- Latin Modern Mono for code blocks (falls back to DejaVu Sans Mono).

On Debian/Ubuntu, both come from TeX Live and the standard font packages:

```sh
sudo apt install texlive-fonts-recommended fonts-liberation fonts-lmodern
```

### Installing Typst

```sh
# Cargo (recommended)
cargo install --locked typst-cli

# or: download a release binary
# https://github.com/typst/typst/releases
```

## Compile

```sh
typst compile slides.typ
```

Output: `slides.pdf` (10 pages, 4:3, 12.8 × 9.6 cm — matches Beamer's
`aspectratio=43`).

## Live preview

Typst's incremental compiler re-renders only what changed, so editing
feels instant:

```sh
typst watch slides.typ
```

`typst watch` only writes the PDF — open it in a viewer that auto-reloads
(e.g. `zathura`, `evince`, `Skim`, or VS Code's Tinymist / Typst LSP
extension which gives a live preview pane and doesn't need `watch` at all).

## Project layout

```
.
├── cscs.typ            # template: colors, slide helpers, blocks, code listings
├── slides.typ          # deck content
├── slides.pdf          # compiled output
├── ref.bib             # BibTeX entries for citations
├── cscs.csl            # custom CSL: "[N] Title. URL." (no "[Online]")
├── README.md
└── images/             # logos and hero images
    ├── alps_smaller.jpg   # title-slide hero (optional)
    ├── image1.pdf         # ETH Zürich logo (top-right + footer)
    ├── image2.pdf         # small CSCS footer mark
    ├── image3.pdf         # title-slide hero (default)
    ├── image4.pdf         # full CSCS logo (title / chapter / thank-you)
    ├── image7.pdf         # thank-you formulas hero
    └── image8.jpeg        # building photo (table of contents)
```

## Editing the deck

The template exposes a small API in `cscs.typ`:

| Helper | Use |
| --- | --- |
| `cscs-presentation(...)` | document setup; called once via `#show: cscs-presentation.with(...)` |
| `cscs-title(...)` | title slide with hero image |
| `cscs-chapter(title)` | section divider slide |
| `cscs-thankyou(message: ..., picture: ...)` | closing slide |
| `cscs-toc(title: ...)[body]` | TOC layout (image left, outline right) |
| `cscs-slide(title: ..., body)` | regular content slide (handles page break + frame title) |
| `cscs-list[...]`, `cscs-enum[...]` | CSCS-styled bullet / numbered lists; nested levels auto-shrink to 0.9× |
| `<color><N>block(title, body)` | colored boxes — `red`, `blue`, `green`, `brown`, `purple`, `yellow`, `black`; `N` is the title-bar variant `0`/`1`/`2` |
| `cscs-listing(language: ..., title: ..., code)` | code block with title bar; uses Typst's built-in syntax highlighter |
| `cscs-inline-code(code, language: "C/C++")` | inline highlighted code |

Recognized listing languages: `C/C++`, `C++`, `C`, `Python`, `Bash`, `Rust`,
`Fortran` (Fortran is detected but Typst doesn't highlight it — it ships
without a Fortran grammar). Any other identifier you pass is lowercased and
forwarded to syntect, so most Sublime-supported languages work.

### Adding a slide

```typst
#cscs-slide(title: "My new slide")[
  Some content here.

  #blue2block("A note")[
    Use any of the colored block helpers.
  ]
]
```

### Switching the title-slide image

Pick one of the photos in `images/` (or add your own) and pass its path
to `cscs-title`:

```typst
#cscs-title(
  picture: images + "/image3.pdf",
  title: "...",
  subtitle: "...",
  author: "...",
  date: "...",
)
```

### Changing the footer text

```typst
#show: cscs-presentation.with(
  ...,
  footer: "My Conference 2026",
)
```

### Citations and references

`ref.bib` holds BibTeX entries. Cite them inline:

```typst
CSCS website #cite(<cscs_website>)
```

The references slide renders the bibliography with a small custom CSL
style (`cscs.csl`) that produces `[N] Title. URL.` — no "[Online]"
boilerplate. URLs are colored in `cscs-blue` to match the brand palette.

To use a built-in style instead (IEEE, APA, Chicago, …) replace the
`style:` argument in `slides.typ`:

```typst
#bibliography("ref.bib", title: ..., style: "ieee")
```

## License

© ETH Zürich, 2026.
