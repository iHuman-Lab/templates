# iHuman Lab Templates

Reusable [Quarto](https://quarto.org/) starter templates in the iHuman Lab's visual theme (orange `#EC672C` accent, Rubik font), for producing consistent talks, posters, and other outputs across the lab.

## Templates

| Template | Use it for | Output |
|---|---|---|
| [`presentation/`](presentation/) | Outreach talks, workshops, conference presentations | HTML slide deck ([reveal.js](https://quarto.org/docs/presentations/revealjs/)) |
| [`poster/`](poster/) | Conference and symposium poster sessions | Print-ready PDF ([Typst](https://typst.app/)) |

Each folder is self-contained with its own `README.md`, quick-start instructions, and assets. To use one:

1. Copy the whole template folder and rename it for your project.
2. Follow the quick-start steps in that folder's `README.md`.
3. Render with `quarto render` (or `quarto preview` for live-reload while editing).

Quarto bundles both reveal.js and Typst internally, so no separate LaTeX install is needed for either template.

## Submitting Finished Work

Finished decks and posters are collected in a separate repo, [iHuman-Lab/presentations](https://github.com/iHuman-Lab/presentations), rather than in this one — this repo stays just the canonical templates. See [SUBMITTING.md](SUBMITTING.md) for how to submit a pull request there.

## Adding a New Template

Keep new templates consistent with the existing ones:

- One self-contained folder per template, with its own `README.md` and `assets/` subfolder.
- Reuse the lab's color system (`#EC672C` primary accent, `#757575` secondary/gray) and Rubik font where the output format supports it.
- Document quick-start steps, file layout, and how to re-theme, the same way [`presentation/README.md`](presentation/README.md) and [`poster/README.md`](poster/README.md) do.

## License

[MIT](LICENSE)
