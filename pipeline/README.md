# Pipeline — LLM-in-the-loop updates

This pipeline keeps the book current as Claude Desktop changes. It runs on a daily schedule, watches Anthropic's official sources, and opens a pull request when something looks new.

**A human always reviews before anything ships.** The pipeline drafts; people decide.

## What it does

```
                    daily cron (GitHub Actions)
                              │
                              ▼
           ┌──────────────────────────────────┐
           │  fetch_sources.py                │
           │  • docs.anthropic.com release    │
           │    notes for Claude apps         │
           │  • anthropic.com/news (RSS)      │
           └──────────────────────────────────┘
                              │
                              ▼
           ┌──────────────────────────────────┐
           │  detect_changes.py               │
           │  diff vs. state.json             │
           │  → no diff? exit 0               │
           └──────────────────────────────────┘
                              │  changes found
                              ▼
           ┌──────────────────────────────────┐
           │  draft_updates.py (Claude API)   │
           │  • drafts changelog/<date>.md    │
           │  • lists chapters likely         │
           │    affected, with notes          │
           │  • flags stale screenshots       │
           └──────────────────────────────────┘
                              │
                              ▼
           ┌──────────────────────────────────┐
           │  GitHub Action opens a PR        │
           │  ← HUMAN REVIEW GATE →           │
           │  reviewer verifies, retakes      │
           │  screenshots, edits prose        │
           └──────────────────────────────────┘
                              │
                              ▼
                    merge → site rebuilds
```

## Why a human gate

The pipeline is fast. It is not careful. It will:

- **Hallucinate features** that don't exist
- **Miss UX nuance** — release notes say *what*, not how it feels to use
- **Flag screenshots as stale** but can't retake them
- **Drift in voice** — left alone, prose creeps toward release-note style

The gate exists because all four of these are unfixable without a person.

## Sources, in priority order

1. **[docs.anthropic.com/en/release-notes/claude-apps](https://docs.anthropic.com/en/release-notes/claude-apps)** — the canonical changelog for the chat apps (desktop, mobile, web). Ground truth.
2. **[anthropic.com/news](https://www.anthropic.com/news)** — the *why* and the broader context. Useful for paragraphs that explain motivation.
3. **[support.claude.com release notes](https://support.claude.com/en/articles/12138966-release-notes)** — same content as #1, different surface. Used as a sanity check.

Third-party aggregators (Releasebot, etc.) are *not* used as ground truth — they can lag or get reorganized. They're acceptable as a fallback if Anthropic-owned sources are unreachable.

## Files

- `fetch_sources.py` *(TODO)* — pulls latest from primary sources
- `detect_changes.py` *(TODO)* — compares against `state.json`
- `draft_updates.py` *(TODO)* — calls Claude API with prompt-cached book context
- `prompts/` *(TODO)* — versioned prompt templates
- `state.json` — last-known snapshot of source content (updated by the pipeline on merge)

## Stack

- **GitHub Actions** for cron + PR mechanics (free for public repos)
- **Python + Anthropic SDK** with [prompt caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) — book content barely changes between runs, so cache hits keep the bill small
- **mdBook** (or similar) builds the published site from `book/` + `changelog/`

## Status

Not yet implemented. The scheduled workflow is currently `workflow_dispatch`-only (manual) — see `.github/workflows/daily-check.yml`.
