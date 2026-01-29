# Payment Dictionary for Idiots

A learning resource for payment industry terminology, compiled as a beautiful LaTeX dictionary with automated PDF generation.

## 📚 About

This project aims to demystify payment industry jargon by creating a comprehensive, easy-to-understand dictionary. Whether you're new to payments or just need a quick reference, this dictionary explains complex concepts in simple terms.

**Target audience:** Beginners, students, developers, and anyone learning about the payment industry.

### Unique Approach: Cross-Domain Analogies

This dictionary uses an innovative approach to make abstract payment concepts intuitive by drawing parallels to **biological and medical systems**. Payment systems are real-time, irreversible, high-risk systems---remarkably similar to how living organisms operate.

For example:
- **EMV chip cards** are explained through immune checkpoint systems
- **Payment switches** are compared to neural routing centers
- **Fraud detection** parallels immune system responses

These analogies help you understand not just *what* payment mechanisms do, but *why* they exist and *how* they protect the system.

## 🚀 Getting the Dictionary

### Download Latest PDF

The dictionary is automatically compiled on every update:
- **Latest version**: [Download from GitHub Actions artifacts](../../actions)
- **Stable releases**: [Download from Releases](../../releases)

## 📖 Current Status

- **Format**: LaTeX → PDF
- **Terms**: Growing to ~100 payment industry terms
- **Style**: Clean and readable (vintage Oxford styling coming later)
- **Features**: Index, bibliography, cross-references

## 🤝 Contributing

We welcome contributions! To add a payment term:

1. See the detailed guide in [CONTRIBUTING.md](docs/CONTRIBUTING.md)
2. Use the template in [examples/sample_entry_template.tex](examples/sample_entry_template.tex)
3. Add your term to the appropriate `latex/entries/*_entries.tex` file
4. Submit a pull request

The PDF will automatically compile and show up in the PR for review.

## 🛠️ Building Locally

Requires Docker:

```bash
docker run --rm -v "$PWD:/work" -w /work/latex texlive/texlive:latest \
  sh -c "pdflatex payment_dictionary.tex && biber payment_dictionary && \
         makeindex payment_dictionary.idx && pdflatex payment_dictionary.tex && \
         pdflatex payment_dictionary.tex"
```

Output: `latex/payment_dictionary.pdf`

## 📁 Project Structure

```
latex/
├── payment_dictionary.tex    # Main document
├── preamble.tex              # Package configuration
├── styles/                   # Custom LaTeX styling
├── frontmatter/              # Title, preface, how-to
├── entries/                  # Dictionary terms (a-z)
└── backmatter/               # Abbreviations, bibliography

examples/
└── sample_entry_template.tex # Template for new terms

docs/
└── CONTRIBUTING.md           # Contribution guidelines
```

## 📝 License

Open source learning resource. Feel free to use, modify, and share.

## 🎯 Roadmap

- [x] Basic LaTeX structure
- [x] GitHub Actions automation
- [ ] Add ~100 payment terms
- [ ] Vintage Oxford dictionary styling
- [ ] Two-column layout
- [ ] Web version (HTML export)
