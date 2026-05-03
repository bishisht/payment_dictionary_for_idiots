# Payment Dictionary for Idiots - Context & Guidelines

This document provides essential context and instructions for AI agents and developers working on the **Payment Dictionary for Idiots** project.

## Project Overview

A LaTeX-based learning resource designed to demystify payment industry terminology. It uses an innovative approach of **cross-domain analogies** (primarily biological and medical) to make abstract payment concepts intuitive.

- **Primary Output:** A high-quality PDF dictionary.
- **Core Philosophy:** Explain *why* mechanisms exist using relatable parallels (e.g., payment switches as neural routing centers).
- **Tech Stack:** LaTeX (TeXLive), Biber (bibliography), MakeIndex (indexing), Docker (local environment), and GitHub Actions (CI/CD).

## Project Structure

```text
latex/
├── payment_dictionary.tex    # Main orchestration file
├── preamble.tex              # Package imports and global config
├── styles/
│   └── dictionary_style.sty  # Custom commands (\dictentry, \analogy, etc.)
├── entries/                  # Alphabetical content (a_entries.tex to z_entries.tex)
├── frontmatter/              # Title page, preface, how-to guide
└── backmatter/               # Abbreviations, bibliography, index
examples/
└── sample_entry_template.tex # Template for new terms
docs/
└── CONTRIBUTING.md           # Detailed contribution guidelines
```

## Key Commands & Workflow

### Building the PDF Locally

The project uses Docker to ensure a consistent LaTeX environment. Three passes are required to resolve references, bibliography, and index.

```bash
# Run from the project root
docker run --rm -v "$PWD:/work" -w /work/latex texlive/texlive:latest \
  sh -c "pdflatex payment_dictionary.tex && \
         biber payment_dictionary && \
         makeindex payment_dictionary.idx && \
         pdflatex payment_dictionary.tex && \
         pdflatex payment_dictionary.tex"
```

### Adding a New Term

1.  **Identify File:** Find the corresponding `latex/entries/{letter}_entries.tex` file.
2.  **Use Template:** Copy the format from `examples/sample_entry_template.tex`.
3.  **Format Entry:** Use the `\dictentry` command:
    ```latex
    \dictentry{Term}{/pronunciation/, \textit{POS}}{
      Definition text.
      \example{Usage example.}
      \analogy{Biological/Medical}{Analogy text.}
      \crossref{\textbf{Related Term}}
    }
    \indexentry{Term}
    ```
4.  **Escape Special Characters:** Always escape LaTeX reserved characters: `& % $ # _ { }` (e.g., `&` becomes `\&`).

## Development Conventions

-   **Branching:** Always work in feature branches (e.g., `feature/add-emv-term`). Never commit directly to `main`.
-   **Alphabetization:** Maintain strict alphabetical order within the entry files.
-   **Plain Language:** Definitions must be accessible to beginners. Avoid using jargon to define jargon.
-   **Analogies:** Highly encouraged. Compare payment systems (high-risk, real-time, irreversible) to biological systems (immune response, neural paths).
-   **Validation:** After adding a term, compile locally to ensure no LaTeX errors and that the term appears in the index.

## Style Guide (Custom Commands)

-   `\dictentry{name}{meta}{content}`: Main entry structure.
-   `\example{text}`: Italicized usage examples.
-   `\analogy{type}{text}`: Cross-domain parallels.
-   `\subdef{n}`: Numbered sub-definitions for polysemous terms.
-   `\crossref{terms}`: Bolded references to other entries.
-   `\indexentry{term}`: Required for the term to appear in the back-of-book index.
