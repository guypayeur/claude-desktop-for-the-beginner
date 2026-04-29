# Contributing

This book is for absolute beginners using the Claude desktop app. The bar for any contribution is: **does this make the book easier to follow for someone who has never used Claude before?**

## Easy ways to help

- **Spotted a typo or unclear sentence?** Open a PR with the fix.
- **Found something outdated** (a screenshot, a button label, a feature that no longer works that way)? Open an issue with the chapter and what changed.
- **Have a better way to explain something?** Open a PR.

## Style guide

- **Plain English.** No jargon without an explanation. If you must use a term, explain it the first time and link to the glossary.
- **Show, don't tell.** Screenshots beat paragraphs. Save them to `book/images/` named `<chapter>-<topic>-vX.png` (e.g., `02-install-windows-v1.5354.png`).
- **One idea per heading.** If a section grows past ~400 words, it probably wants to split.
- **Second person.** Write to *you*, not to *the user*.
- **No condescension.** The reader is new to this app, not new to thinking.

## The auto-update pipeline

Most chapter updates come in via the LLM-in-the-loop pipeline (see [pipeline/README.md](pipeline/README.md)). When you review an auto-generated PR:

- Verify every claim against the actual app — LLMs hallucinate features.
- Retake screenshots if the UI moved. The pipeline can flag stale shots but can't replace them.
- Adjust the prose to match the book's voice; release-note language is too terse.

## Code of conduct

Be kind. Disagree about content, not people.
