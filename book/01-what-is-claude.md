---
chapter: 1
title: What is Claude, and what's the desktop app?
status: draft
---

# What is Claude, and what's the desktop app?

If you've made it to this page, you've probably already heard the word *Claude* enough times to be curious. Maybe a colleague keeps mentioning it. Maybe it came up in a meeting. Maybe a screenshot floated through your feed.

This chapter is the answer to: *okay, so what is it, actually?* — without the marketing copy and without assuming you've used anything like it before.

## Claude in 60 seconds

Claude is an AI assistant. You type something — a question, a request, a half-formed thought — and Claude writes something back. It's made by a company called Anthropic.

The closest comparison most people reach for is a chatbot, but that undersells it. A more useful comparison is a thoughtful colleague who happens to have read most of the public internet, can answer in every major language, and will work on almost anything you put in front of them: drafting an email, summarizing a 60-page PDF, planning a trip, untangling a spreadsheet, or just helping you think out loud.

It is not a search engine. Search engines find pages other people wrote. Claude composes a fresh answer for you, in your context, on the spot. That's a feature when you need synthesis. It's a problem when you need a verifiable source — a topic [Chapter 13](13-when-claude-gets-it-wrong.md) is entirely dedicated to.

## The Claude family in 60 seconds

When people say *Claude*, they don't always mean the same thing. There is one AI underneath, but it shows up through several different doors — and a couple of those doors are for software developers, not for everyone else. Before we go any further, here is the map.

For ordinary use — chatting, asking questions, working with documents — Claude lives in three places:

- **The website** at `claude.com`
- **The desktop app** for Windows and Mac (this book)
- **The mobile app** for phone and tablet

For software developers building their own things, there are two more, and they are mentioned here only so you can recognize the names if they come up:

- **Claude Code** — a coding assistant that lives in a developer's terminal. Useful if you write code professionally; safely ignorable if you don't.
- **The Claude API** — a way for programmers to plug Claude into apps and services they're building. Also a developer thing.

If you have never written code in your life, the three chat doors above are the ones that matter to you, and this book covers the second of them. The two developer doors are mentioned only so you can recognize the names if they come up — they are different products with different audiences, not hidden features of the desktop app.

Worth saying out loud, though: the line between "people who write code" and "people who don't" is moving fast. With Claude on your screen, you can describe what you want a small program to do, in plain words, and watch it get written — a one-off script that renames a folder full of files, a tiny web page for a project you've been putting off, a spreadsheet macro you'd never have asked anyone to build for you. None of those require Claude Code or any developer setup; the desktop app you're reading about is enough to make them happen. Maybe code is just what was missing in the Beginner in All of Us.

## The desktop app vs. the website vs. the mobile app

There is one Claude. It just lives in three different containers.

- **The website** — `claude.com`, in any web browser. Nothing to install. The easiest way to try Claude for the first time.
- **The desktop app** — what this book is about. You install it on your Windows PC or your Mac and it runs in its own window like any other app. Same Claude as the website, with extras you can't easily get in a browser tab: it can drive your browser for you, take dictation more cleanly, sit in its own window without competing with your tabs, and quietly handle longer tasks in the background.
- **The mobile app** — for phone and tablet. Same Claude again. Better for short questions on the go, less suited to anything where you'd want a real keyboard.

If you spend most of your day on a computer, the desktop app is almost certainly the one you want. The rest of this book assumes you have it installed; if you don't yet, [Chapter 2](02-installing.md) walks through it.

## What this book covers

This book is about the **Claude desktop app on Windows and Mac** — specifically the consumer app, which is officially branded *Claude for Windows* or *Claude for Mac* depending on which you have. (The phrase "Claude Desktop" in the title of this book is the name everyone actually uses out loud, even though it isn't on the app's About page.)

The mobile app is not covered here — different screen, different gestures, different habits — and may grow into its own book one day. The two developer products mentioned above (Claude Code and the API) aren't covered either.

Inside its scope, this book is comprehensive but unhurried. We move from "where do I click" through "what does this button actually do" to "when should I trust the answer." If you read it front to back, you'll know everything a beginner needs. If you skim and skip, the [table of contents](SUMMARY.md) is built so you can jump in anywhere.

## A first look at the app

When you open Claude for the first time, after signing in (covered in [Chapter 2](02-installing.md)), you'll see something close to this:

- **A big empty box in the middle of the window.** This is where you type. It's the first thing you'll touch and you'll keep coming back to it.
- **A list down the left side.** Once you've had a few conversations, this is where they live. Each one is a separate *chat*. You can return to any of them, rename them, or delete them.
- **A "+" or "New chat" button.** Starts a fresh, blank conversation. Use this generously — fresh chats are cheap, and Claude tends to do better when each conversation has a clear purpose.
- **Your name or initials in a corner.** Click here for settings, your account, billing, and sign-out.
- **A model name near the top of the chat.** Something like *Claude Opus 4.7* or *Claude Sonnet 4.6*. Different *models* are different versions of Claude with different strengths. For now, the default is the right choice and you can ignore this control. We'll come back to it.
- **A "Customize" section, usually accessible from a side panel.** This is where Skills, Connectors, and other extensions live. Three later chapters explain what those are and why you'd want them. For now you don't need to touch any of it.

That is the entire app, at first glance. It looks deceptively simple — almost suspiciously simple, given everything Claude can do. That's intentional. Most of Claude's surface area is hidden behind that empty box, waiting for you to ask for it.

## What's coming next

[Chapter 2](02-installing.md) gets the app onto your computer. [Chapter 3](03-first-conversation.md) gets you through your first useful conversation. From there we work outward — files, projects, voice, and then the more powerful features.

A small word of advice before you start: don't try to learn Claude by reading about it. The fastest way to understand what Claude is good at is to put a real problem in front of it and see what happens. This book is here to make that easier and to point you at things you might not discover on your own — but the conversations themselves are still the point.
