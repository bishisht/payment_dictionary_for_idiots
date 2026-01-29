# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Payment Dictionary for Idiots** - A LaTeX-based learning resource that compiles payment industry terminology into a dictionary PDF. The project uses GitHub Actions for automated compilation.

## Technology Stack

- **LaTeX**: Document typesetting (TeXLive distribution)
- **BibLaTeX + Biber**: Bibliography management
- **MakeIndex**: Automated index generation
- **GitHub Actions**: CI/CD for PDF compilation
- **Docker**: Local testing with `texlive/texlive:latest`

## Development Workflow

### Adding a New Payment Term

1. Determine alphabetical file: `latex/entries/{letter}_entries.tex`
2. Copy template from `examples/sample_entry_template.tex`
3. Fill in: term, pronunciation, definition, example, cross-refs
4. Add `\indexentry{TERM_NAME}` for index inclusion
5. Commit to feature branch (never to main)

### Building the PDF Locally

```bash
cd latex
docker run --rm -v "$PWD:/work" -w /work texlive/texlive:latest \
  sh -c "pdflatex payment_dictionary.tex && \
         biber payment_dictionary && \
         makeindex payment_dictionary.idx && \
         pdflatex payment_dictionary.tex && \
         pdflatex payment_dictionary.tex"
```

**Why multiple passes:**
- First `pdflatex`: Generate auxiliary files
- `biber`: Process bibliography
- `makeindex`: Build index
- Second/third `pdflatex`: Resolve all references

### Testing Changes

After adding/modifying entries:
1. Run local compilation (above command)
2. Check `payment_dictionary.pdf` for formatting
3. Verify: entry appears correctly, index includes term, cross-refs work
4. Check compilation logs for warnings/errors

### GitHub Actions

- Triggers: Push/PR to main with changes in `latex/**`
- Output: Artifact `payment-dictionary-pdf` (available for 90 days)
- Releases: Tag with `v*` creates GitHub Release with PDF
- Logs: Check Actions tab for LaTeX compilation errors

## Project Architecture

### File Organization

```
latex/
├── payment_dictionary.tex          # Main orchestration file
├── preamble.tex                    # All package imports & config
├── styles/dictionary_style.sty     # Custom \dictentry commands
├── frontmatter/                    # Title, preface, intro
│   ├── titlepage.tex
│   ├── preface.tex
│   └── howto.tex
├── entries/                        # Alphabetical content files
│   ├── a_entries.tex               # Terms: A*
│   ├── b_entries.tex               # Terms: B*
│   └── ...                         # (one file per letter)
├── backmatter/                     # End matter
│   ├── abbreviations.tex
│   └── references.bib              # BibTeX bibliography
```

### Custom LaTeX Commands (in `dictionary_style.sty`)

- `\dictentry{term}{pronunciation/POS}{definition}` - Main entry
- `\subdef{number}` - Numbered sub-definitions
- `\example{text}` - Usage examples (italicized)
- `\crossref{terms}` - Related term references
- `\etymology{origin}` - Historical notes
- `\indexentry{term}` - Add to alphabetical index

### Style Evolution

**Current (MVP):** Basic, clean formatting for readability and reliability
- Single column
- Standard fonts
- Clear typography
- Focus: content structure

**Future (Vintage Phase):**
- Two-column layout
- Aged paper aesthetic (cream background)
- Classic serif fonts (Times-style)
- Running headers (guide words)
- Drop caps at letter sections
- Compact spacing (1970s Oxford style)

## Common Tasks

### Add Multiple Terms at Once

1. Create feature branch: `git checkout -b add-card-terms`
2. Edit multiple `latex/entries/*.tex` files
3. Test compilation locally
4. Commit with descriptive message: "Add 10 card payment terms (authorization, capture, etc.)"
5. Push and create PR

### Fix LaTeX Compilation Error

1. Check GitHub Actions logs for error line
2. Common issues:
   - Special characters need escaping: `\& \% \$ \# \_ \{ \}`
   - Unclosed braces: ensure `{` has matching `}`
   - Missing `\indexentry` after `\dictentry`
3. Test fix locally before pushing

### Update Styling (dictionary_style.sty)

1. Modify `latex/styles/dictionary_style.sty`
2. Changes affect ALL entries automatically
3. Test with full compilation (3x pdflatex)
4. Check multiple entry types (simple, multi-def, with examples)

### Add New Bibliography Source

1. Edit `latex/backmatter/references.bib`
2. Use standard BibTeX format (online, book, article)
3. Cite in entries: `\cite{key}`
4. Run `biber` to process

## Conventions

### Definition Writing

- Plain language (avoid jargon in definitions)
- Active voice preferred
- Real-world examples from payment industry
- Cross-reference related terms generously
- Typical structure: "Definition. Context. \example{Usage.}"

### Pronunciation Guide

- IPA preferred: `/eɪ siː eɪtʃ/` for ACH
- Phonetic acceptable: `/ay see aych/`
- Part of speech after pronunciation: `\textit{abbr.}`, `\textit{n.}`, `\textit{v.}`

### Alphabetization

- Strict alphabetical order within each letter file
- Acronyms: alphabetize by letters (ACH under A)
- Ignore case for sorting (APIs comes after API)

## Special Character Escaping

LaTeX requires escaping for these characters:
- `&` → `\&`
- `%` → `\%`
- `$` → `\$`
- `#` → `\#`
- `_` → `\_`
- `{` → `\{`
- `}` → `\}`
- `~` → `\textasciitilde{}`
- `^` → `\textasciicircum{}`

## Future Enhancements

Planned for post-100-term milestone:
- Two-column layout with `multicol`
- Vintage typography (aged paper, classic fonts)
- Running headers with guide words (first/last term per page)
- Drop caps at section starts
- Enhanced cross-reference navigation
- Web HTML version export

## Notes for Claude Code

- **Always work in feature branches**, never commit directly to main
- **Test LaTeX compilation** after any changes to .tex files
- **Validate template usage**: ensure new entries follow `\dictentry` format
- **Check for special characters**: LaTeX requires escaping
- **Three compilation passes required** for complete PDF (refs + index)
- User will provide payment terms incrementally - make adding terms trivial
- Prioritize: ease of contribution over complex features
