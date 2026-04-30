# Outline — full table of contents

The detailed planning document for the book. Section-by-section breakdown of every chapter, used as the writing plan and as input to the LLM-in-the-loop pipeline (so it knows where each new feature most likely belongs).

For reader navigation, see [SUMMARY.md](SUMMARY.md).

---

## Foreword

- Who this book is for
- What this book isn't (the API, Claude Code, the mobile app)
- How to read it
- A note on how this book stays current

## Part I — Getting going

### 1. What is Claude, and what's the desktop app?

- Claude in 60 seconds
- The desktop app vs. the website vs. the mobile app
- What this book covers (and what it doesn't)
- A quick tour of what you'll see when you open it for the first time

### 2. Installing Claude

- On Windows: download, install, first launch
- On Mac: download, install, first launch
- Signing in — Free, Pro, Max: what changes at install time
- "It won't open" / "It won't sign in" — common fixes
- Updating Claude (and how the app updates itself)

### 3. Your first conversation

- The empty box, explained
- A worked example, end to end
- Why the answer might surprise you (Claude is generative, not search)
- Stopping, retrying, and starting over

## Part II — The basics

### 4. How a conversation actually works

- Turns, threads, and what "context" means
- What Claude remembers within one chat
- What Claude forgets when you start a new chat
- Editing earlier messages vs. starting fresh
- The little buttons: regenerate, copy, share

### 5. Dropping files into Claude

- What you can drop in (PDFs, docs, sheets, images, code)
- What Claude actually does with the file
- Limits — file size, page counts, what tends to fail
- Multiple files in one conversation
- Pictures specifically — what Claude can and can't see in an image

### 6. Projects: keeping your work organized

- What a Project is, and what it isn't
- When making one earns its keep
- Custom instructions — telling Claude how to behave inside this project
- Reference files — the "always available" docs in a Project
- Sharing a Project with others (if your plan supports it)

### 7. Voice and dictation

- Talking to Claude instead of typing
- Hearing Claude back
- When voice helps, and when it gets in the way
- Accents, languages, and how well it copes
- Privacy — what's processed where

## Part III — Power moves, gently

### 8. Skills

- What a Skill is, in plain terms
- Skills that are turned on by default
- The Customize section — where to find them
- Turning a Skill on, off, and back on
- Skills that beginners actually use — a short tour

### 9. Connectors

- What a Connector is
- The ones a beginner is most likely to want (Calendar, Drive, Slack…)
- Setting one up, step by step
- What gets shared, with whom — privacy in plain terms
- Disconnecting, in case you change your mind

### 10. Browser actions

- What Claude can do in a browser
- A worked example — looking up something across two sites
- When this earns its keep, and when it's overkill
- Recording a workflow — showing Claude something once
- What can go wrong (and how to recover)

### 11. Cowork

- What Cowork is, and why it matters
- The kinds of tasks worth handing off
- Watching Claude work on your behalf
- Stopping a task that's gone sideways
- The honest tradeoffs of letting an agent run

## Part IV — Living with Claude

### 12. Privacy, memory, and what Claude keeps

- Memory across conversations — what's in there, and how to see it
- Removing things from memory
- Turning memory off entirely
- What Anthropic sees, and what it uses for training
- The settings page worth a five-minute audit

### 13. When Claude gets it wrong

- Hallucinations, in plain terms
- Confident-but-wrong — how to spot it
- Asking for sources (and when to trust them)
- Editing the prompt vs. starting over
- When the answer is right but unhelpful — redirecting tone

### 14. Plans, limits, and what costs what

- Free, Pro, Max — who each is for
- What "rate limits" actually mean for you
- Upgrading, downgrading, cancelling
- The features that change with the plan
- A note on pricing — the pipeline flags this chapter whenever Anthropic updates its plans page

## Appendices

### A. Glossary

Plain-English definitions for the terms used in the book: Skill, Connector, Project, Cowork, hallucination, rate limit, context window, memory, and so on.

### B. Keyboard shortcuts

A printable one-page cheat sheet for Windows and Mac.

### C. Changelog archive

Links to every "what changed in version X" post, newest first.

### D. Where to get help

- Anthropic's official help center
- This repo's issue tracker (for things in this book)
- Community forums (Reddit, Discord, etc.) — with caveats about quality

---

## Conventions used in this outline

- **Sections are bullet points, not chapter pages.** Each bullet is a heading inside the chapter file, not a separate file.
- **3–6 sections per chapter** is the target. Past 6, the chapter probably wants to split.
- **No section is a stub for "TODO".** Every bullet is a real thing the chapter teaches.
