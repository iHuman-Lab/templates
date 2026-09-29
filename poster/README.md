# iHuman Lab Poster Template

A [Quarto](https://quarto.org/) + [Typst](https://typst.app/) academic poster starter in the lab's visual theme (white background, orange `#EC672C` accent, Rubik font). Used for conference and symposium poster sessions.

It's built on the community [`quarto-ext/typst-templates` poster extension](https://github.com/quarto-ext/typst-templates/tree/main/poster), which wraps Parth Parikh's [typst-poster](https://github.com/pncnmnp/typst-poster) package (MIT-licensed) — a real academic poster layout (logo + title/author header, footer bar, numbered sections), rather than a plain document reflowed into columns. The package is vendored under `_extensions/poster/` and lightly patched for the lab's branding — see [Branding Patch](#branding-patch) below.

Quarto ships Typst internally, so there's no separate LaTeX or Typst install — `quarto render` is all you need.

## Quick Start

1. Copy this whole `poster/` folder and rename it for your poster (e.g. `my-neurips-poster/`).
2. Edit the frontmatter at the top of `template.qmd`: `title`, `poster-authors`, `departments`, `footer-*`, `keywords`.
3. Replace `assets/logo.png` if you want different art (keep the same filename, or update `institution-logo` in the frontmatter).
4. Fill in your own sections, reusing the patterns already in the file.
5. Render:

   ```bash
   quarto render template.qmd     # build once -> template.pdf
   quarto preview template.qmd    # live-reload while editing
   ```

The output is a PDF sized for large-format printing (36in × 24in landscape by default — a common conference poster-board size) — send it straight to your university's poster printing service, or a local print shop.

Keep roughly as much placeholder text as the template ships with. Typst lays the body out in newspaper-style columns that fill **sequentially** (column 1 top-to-bottom, then column 2, …), not balanced like a word processor — so a poster with much less text than the template will leave later columns empty, and one with more may spill onto an unwanted second page. If your content doesn't fill the poster once you're done, either add more (a related-work paragraph, an extra figure) or shrink `size` in the frontmatter; if it overflows, trim text, shrink a figure's `width`, or combine two `##` sub-headings under fewer parent sections to save the spacing each heading adds.

## File Layout

```
poster/
├── template.qmd          # the poster itself — copy & edit this
├── README.md              # this file
├── assets/
│   ├── logo.png             # iHuman Lab logo, placed in the header
│   └── figure-placeholder.svg  # placeholder art for the two demo figures — replace with your own
└── _extensions/poster/     # the vendored quarto-ext/typst-templates extension
    ├── _extension.yml
    ├── typst-show.typ       # forwards template.qmd's YAML into poster()
    └── typst/packages/local/typst-poster/0.1.1/
        ├── poster.typ        # the poster layout itself — branding patch lives here
        ├── typst.toml
        └── LICENSE           # typst-poster's MIT license
```

You shouldn't need to touch anything under `_extensions/` when writing a poster — only `template.qmd` and `assets/`.

## Layout Model

Unlike the presentation template (one slide per `##`), the poster is a real academic-poster layout: a header row (logo, title, authors, department), a numbered multi-column body, and a footer bar. Level-1 (`#`) headings are the poster's major sections (Introduction, Methods, Results, …) and get a Roman numeral, a colored heading, and a horizontal rule; level-2 (`##`) headings are run-in sub-headings within a section (e.g. "Objectives" under "Introduction").

| Element | What it's for | How |
|---|---|---|
| `# Heading` | A numbered, colored major section | Start a top-level section |
| `## Heading` | An italic orange run-in sub-heading | Nest under a `#` section |
| `**text**` | Bold emphasis | Standard markdown, anywhere |
| Markdown tables | Results tables, with an auto-numbered caption | Standard `\| col \| col \|` markdown, with `: Caption {#tbl-id}` underneath |
| `![caption](assets/figure.png){width="100%"}` | A captioned figure | Add the image to `assets/`, then reference it — see the commented-out example under "Methods" |
| `keywords:` (frontmatter) | A "Keywords — ..." line at the top of the body | A YAML list, e.g. `["term one", "term two"]` |
| `::: {.block fill="..." inset="..." radius="..." stroke="..."}` | A boxed callout (e.g. a "key takeaway" highlight) — see the Conclusion section | Quarto's native Typst passthrough: a `.block` div's attributes are forwarded straight to Typst's `#block(...)`. Colors need Typst's `rgb(r,g,b)` decimal form (no nested quotes) — the lab's orange `#EC672C` is `rgb(236,103,44)`, its light gray `#DDDDDD` is `rgb(221,221,221)` |

## Poster Options

Set these under `format.poster-typst` in `template.qmd`'s frontmatter:

| Key | What it controls |
|---|---|
| `size` | Poster dimensions as `"widthxheight"` in inches, e.g. `"36x24"` — swap the two for portrait. Tested by upstream on 36×24, 48×36, and 36×48. |
| `poster-authors` | Comma-separated author names |
| `departments` | Department/institution line shown next to the authors |
| `institution-logo` | Path to the lab/university logo, shown on the **right** of the header (must start with `/` — see [Branding Patch](#branding-patch)) |
| `univ-logo-scale` | Lab logo width as a percentage of its column |
| `venue-logo` / `venue-logo-scale` | Same, but for a conference/venue logo on the **left** of the header — empty by default; commented out in `template.qmd` |
| `univ-logo-column-size` / `title-column-size` | Header column widths, in inches — keep the logo columns narrow (the template ships with `"5"`) so each logo reads as sitting *beside* the centered title rather than dominating its own row |
| `footer-text` / `footer-url` / `footer-emails` / `footer-color` | The orange footer bar's contents and color (hex, no `#`) |
| `keywords` | YAML list shown at the top of the body |
| `num-columns` | Number of body columns (default 3; 2-column posters may need different size/font settings — see [typst-poster's examples](https://github.com/pncnmnp/typst-poster/tree/main/examples)) |
| `body-font-size` | Body text size, in points (default `"20"` — an iHuman Lab addition, see [Branding Patch](#branding-patch); upstream hardcodes 16pt) |
| `title-font-size` / `authors-font-size` / `footer-*-font-size` | The header/footer's other font sizes, in points |

## Branding Patch

The vendored `_extensions/poster/typst/packages/local/typst-poster/0.1.1/poster.typ` differs from [upstream](https://github.com/pncnmnp/typst-poster/blob/master/poster.typ) in eleven places, each marked with an `// iHuman Lab branding` comment:

1. Body font is `"Rubik"` instead of `"STIX Two Text"` (matching the presentation theme).
2. Section headings and their rule lines are colored `#EC672C` instead of black.
3. The poster title is colored `#EC672C`, bold, and centered (upstream left-aligns it next to the logo).
4. Body text size is a `body-font-size` option (default 20pt) instead of being hardcoded to 16pt.
5. The header is a 3-slot layout (venue logo left, title/authors centered, lab logo right) instead of upstream's 2-column (logo, title) layout — see `venue-logo` above.
6. Section headings request an explicit sans-serif stack (`"Rubik", "Arial", "Helvetica", "Liberation Sans", "DejaVu Sans"`) instead of inheriting whatever the body font resolves to — so headings stay sans-serif even on a machine without Rubik installed, where Typst's own fallback font is a serif. The body text itself still falls back to Typst's default serif in that case (see [Fonts](#fonts)) — install Rubik for a fully consistent look.
7. The gap between a level-1 heading's text and the rule line beneath it is tighter (10pt instead of upstream's ~36pt).
8. Paragraphs use a `leading` (within-paragraph line spacing) of 0.9em instead of Typst's default 0.65em, on top of upstream's paragraph-to-paragraph `spacing`.
9. Tables get an orange header row with bold white text and thin gray gridlines, instead of Typst's default all-black grid — upstream doesn't theme tables at all.
10. The rule under a level-1 heading is an angled accent bar (a diagonal cut on the right end) instead of a plain straight line — replicating the presentation theme's signature `heading-accent` clip-path motif (see `theme.scss`) using a Typst `polygon()`.

It also accepts the university logo as ready-made Typst content (an `#image(...)` call) rather than a path string — Typst sandboxes an imported package's filesystem access to its own package folder, so a path string passed into `poster.typ` would have been resolved from inside `_extensions/poster/typst/packages/local/typst-poster/0.1.1/`, not from this project. `typst-show.typ` builds the `#image("$institution-logo$", ...)` call itself (outside the sandboxed package), which is why `institution-logo` needs a leading `/` — Typst resolves that as project-root-relative, and `resources:` in the frontmatter tells Quarto to copy the file there.

To pull in a newer version of typst-poster: download the new `poster.typ` from upstream, diff it against the current file to find the branding changes above (search `iHuman Lab branding`) and the `univ_logo` content-vs-path change, then reapply them to the new version. Update the version number in `typst.toml` and in the `@local/typst-poster:0.1.1` import line in `_extensions/poster/typst-template.typ` and the folder name under `typst/packages/local/typst-poster/` to match.

## Fonts

The template requests **Rubik** via Typst's system font search. If Rubik isn't installed on your machine, Typst silently substitutes a default serif/sans-serif — the poster still renders correctly, just not in the exact lab font. To get an exact match, install [Rubik from Google Fonts](https://fonts.google.com/specimen/Rubik) system-wide before rendering. (The footer's URL/email are set in `Courier`, upstream's choice for a monospaced look; same fallback behavior applies if it isn't installed.)

## Publishing

`quarto render` produces a single PDF (`template.pdf`) sized for print. To also share it as a link (e.g. on the lab website or in an email), that same PDF works as-is — no separate export step needed.
