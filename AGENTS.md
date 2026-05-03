# Repository Guidelines

## Project Structure & Module Organization
Core content lives under `latex/`. Use `latex/payment_dictionary.tex` as the main orchestrator, with entries split by first letter in `latex/entries/*_entries.tex` (for example, `latex/entries/t_entries.tex`). Shared formatting commands are in `latex/styles/dictionary_style.sty`. Introductory pages live in `latex/frontmatter/`, and references/index inputs live in `latex/backmatter/`. Use `examples/sample_entry_template.tex` when adding new terms. CI automation is defined in `.github/workflows/compile-latex.yml`.

## Build, Test, and Development Commands
From repository root, run a full local build:

```bash
docker run --rm -v "$PWD:/work" -w /work/latex texlive/texlive:latest \
  sh -c "pdflatex -interaction=nonstopmode payment_dictionary.tex && \
         biber payment_dictionary && \
         makeindex payment_dictionary.idx && \
         pdflatex -interaction=nonstopmode payment_dictionary.tex && \
         pdflatex -interaction=nonstopmode payment_dictionary.tex"
```

Output is `latex/payment_dictionary.pdf`. For quick checks, a single `pdflatex` pass is acceptable, but final validation should use the full sequence above.

## Coding Style & Naming Conventions
Follow the template structure:
- `\dictentry{TERM}{/pronunciation/, \textit{part of speech}}{...}`
- Optional helpers: `\example{}`, `\crossref{}`, `\etymology{}`, `\analogy{}{}`
- Required after each entry: `\indexentry{TERM}`

Keep entries in the correct letter file and maintain alphabetical ordering within that file. Use clear, plain-language definitions. Escape LaTeX special characters (`& % $ # _ { }`) and leave one blank line between entries.

## Testing Guidelines
There is no separate unit-test framework; successful LaTeX compilation is the test gate. Before opening a PR, confirm:
- PDF builds without errors.
- New/updated terms render correctly.
- Index and cross-references resolve as expected.

CI runs on changes to `latex/**` for pushes and PRs targeting `main`.

## Commit & Pull Request Guidelines
Use short, imperative commit subjects (for example, `Add cross-domain analogies feature`). Work on feature branches, not `main` (for example, `add-tokenization-term`). PRs should include:
- What changed and why.
- Files/sections touched.
- A note that local compile passed (and screenshots or PDF snippets for layout-sensitive changes).
