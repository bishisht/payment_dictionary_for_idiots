# Contributing to Payment Dictionary

## Adding a New Payment Term

### Step-by-Step Guide

1. **Find the right file**: Locate the alphabetical file for your term
   - Example: "Tokenization" → `latex/entries/t_entries.tex`
   - Example: "ACH" → `latex/entries/a_entries.tex`

2. **Copy the template**: Use `/examples/sample_entry_template.tex` as your starting point

3. **Fill in the details**:
   - **Term name**: Exact spelling and capitalization (e.g., "ACH", "Tokenization")
   - **Pronunciation**: IPA or phonetic (e.g., `/eɪ siː eɪtʃ/` or `/ay see aych/`)
   - **Part of speech**: Use abbreviations (n., v., adj., abbr.)
   - **Definition**: Clear, simple explanation in plain language
   - **Example**: Real-world usage scenario that's relatable
   - **Cross-references**: Link to related terms in the dictionary
   - **Etymology**: Historical context (optional, if relevant)

4. **Add index entry**: Include `\indexentry{TERM_NAME}` after the definition block

5. **Test locally** (optional but recommended):
   ```bash
   docker run --rm -v "$PWD:/work" -w /work/latex texlive/texlive:latest \
     pdflatex payment_dictionary.tex
   ```

6. **Create a feature branch**: Never commit directly to main
   ```bash
   git checkout -b add-tokenization-term
   ```

7. **Commit and push**:
   ```bash
   git add latex/entries/t_entries.tex
   git commit -m "Add Tokenization term definition"
   git push origin add-tokenization-term
   ```

8. **Create Pull Request**: The PDF will auto-compile via GitHub Actions for review

## Writing Good Definitions

### Do's

- **Use plain language**: Explain as if talking to someone with no payment background
- **Provide context**: Why does this term matter in the payment industry?
- **Include practical examples**: Real-world scenarios help understanding
- **Keep it concise**: Aim for 2-4 sentences for the main definition
- **Use active voice**: "ACH processes transactions" not "Transactions are processed by ACH"
- **Link to related concepts**: Help readers explore connected topics

### Don'ts

- **Avoid circular definitions**: Don't use the term to define itself
- **Don't assume knowledge**: Explain acronyms fully on first use
- **Don't use jargon**: If you must use technical terms, define them too
- **Don't be vague**: Be specific about what something does and why it matters

### Example of a Good Definition

```latex
\dictentry{Tokenization}{/tokənɪˈzeɪʃən/, \textit{n.}}{
The process of replacing sensitive payment card data with a unique identifier
called a token. This token can be used for payment processing but is useless
if stolen, because it cannot be reverse-engineered to reveal the original
card number. Tokenization is a key security measure that protects cardholder
data during storage and transmission.

\example{When you save your credit card in a mobile wallet app, the app stores
a token instead of your actual card number. Each time you pay, the token is
sent instead of your real card details.}

\crossref{\textbf{Encryption}, \textbf{PCI DSS}, \textbf{EMV}}

\etymology{From "token" (a thing serving as a symbol or substitute) + "-ization"
(process of making). Became widespread in payments after 2011.}
}
\indexentry{Tokenization}
```

## Style Guidelines

### Formatting

- **Bold** for the term name in the main entry
- *Italic* for pronunciation, part of speech, examples, and cross-references
- Use `\textbf{}` for bold, `\textit{}` for italics in LaTeX
- Maintain consistent spacing (one blank line between entries)

### Pronunciation Guide

Choose one of these formats:

**IPA (preferred):**
- `/eɪ siː eɪtʃ/` for "ACH"
- `/tokənɪˈzeɪʃən/` for "Tokenization"

**Simple phonetic:**
- `/ay see aych/` for "ACH"
- `/toh-ken-ih-ZAY-shun/` for "Tokenization"

### Cross-References

Link to related terms that help build understanding:

```latex
\crossref{\textbf{ACH}, \textbf{Wire Transfer}, \textbf{SWIFT}}
```

Make sure referenced terms exist or will be added to the dictionary.

## Testing Your Changes

### Local Compilation (macOS/Linux)

Requires Docker:

```bash
cd /path/to/payment_dictionary_for_idiots

docker run --rm -v "$PWD:/work" -w /work/latex texlive/texlive:latest \
  sh -c "pdflatex payment_dictionary.tex && \
         biber payment_dictionary && \
         makeindex payment_dictionary.idx && \
         pdflatex payment_dictionary.tex && \
         pdflatex payment_dictionary.tex"
```

The PDF will be generated at `latex/payment_dictionary.pdf`.

### What to Check

After compilation:
- [ ] Your entry appears in the correct alphabetical section
- [ ] Formatting looks clean (no weird line breaks or spacing)
- [ ] Cross-references are properly formatted
- [ ] The term appears in the index at the back
- [ ] No LaTeX compilation errors in the output

## Common Issues and Solutions

### Special Characters

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

### Compilation Errors

If GitHub Actions fails:
1. Check the Actions log for the error line number
2. Common issues:
   - Missing closing brace `}`
   - Unescaped special characters
   - Missing `\indexentry` after `\dictentry`
3. Test locally to debug faster
4. Fix and push again

### Index Not Updating

If your term doesn't appear in the index:
- Make sure you included `\indexentry{TERM_NAME}` after the entry
- Check that the term name matches exactly (case-sensitive)
- Remember: three compilation passes are needed for index to appear

## Questions?

If you're unsure about how to structure a definition or have questions about contributing, feel free to:
- Open an issue on GitHub
- Check the existing entries in `latex/entries/` for examples
- Review the sample template in `examples/sample_entry_template.tex`

Thank you for helping make payment terminology accessible to everyone!
