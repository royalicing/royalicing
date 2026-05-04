---
title: Accessibility-First Tool Concepts
date: 2021-05-07
---
<meta property="og:published_time" content="2021-05-07">

# Accessibility-First Tool Concepts

Twice now I’ve created desktop apps for designing UIs. Neither shipped and I know I want to return to this space again.

My current thinking is that accessibility is a must, and is something tools today severely lack. They are visual-first and often visual-only. Why aren’t we thinking about accessibility at the early design stage?

Doing so would both make implementation easier as we aren’t just bolting accessibility needs at the end. And faster too if we get it right from the beginning.

I think getting developers (tricking them almost) to use accessibility affordances for their own needs (for example writing tests) is an interesting way to get them to care more about it.

Here are some tooling ideas:

## Emulate Screen Reader Output

- Supports VoiceOver, JAWS, NVDA.
- Turns HTML into what would be spoken by a screen reader.
- Can validate what actual screen readers would interpret, without having to run all of them.
- Can use in Snapshot tests to ensure implementations are accessible and don’t break due to changes.

## Screen reader emulator as CLI

- Run with URL.
- It streams back a screen reader representation of the page.
- Actually might be useful for developers for browsing a website.


## OCR automated testing with contrast / colour checking

- Takes text and a role as input.
- Renders using Playwright, and uses OCR to find the element visually on the page.
- Only work if text passes expected contrast and colour requirements. e.g. “Can’t read button ‘Sign Up’ as it lacks contrast”.

## Accessibility-first prototyping tool

- Content is usually what differentiates your brand, what the user reads, what matches user’s existing language.
- Write the content first, and what fundamental accessible widgets you want used.
- See a live preview without writing any code — for visual users and for screen reader users.
- Export a set of automated tests to verify your actual implementation.
