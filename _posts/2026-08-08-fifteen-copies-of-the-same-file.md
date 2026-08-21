---
layout: post
title: "Fifteen copies of the same file"
date: 2026-08-08 20:33:00 -0400
tags: ["Light Phone", "Kotlin"]
description: "The least interesting code I maintain is the code I'd save first in a fire. On finally building the shared library."
---

This is a post about the least interesting code I maintain, which is also the code I'd save first in a fire.

At some point I looked up and had a lot of Light Phone apps. Every one of them needs the same plumbing: handle the hardware keys, drive the scroll wheel, draw text in the phone's type and color system, run the shake-to-report machinery. And every one of them had its own copy. `LightKeys.kt` was byte-identical in fifteen repos. The wheel handling existed in about ten variants across twenty — not because ten variants were needed, but because each copy drifted a little after it was pasted.

You can guess the failure mode. A fix to the wheel meant twenty edits, so the fix got made once, in whichever app I happened to be in, and the other nineteen kept the bug. Multiply by every shared component and the collection was slowly turning into an archaeology site where you could date each app by which bugs it still had.

## The part that actually convinced me

Tidiness alone never gets me to do a refactor this dull. What did it was porting the report module into the older apps by hand — the same feature, wired into ten repos by pattern-matching against a reference implementation. Eight of the ten ended up subtly wrong. Not broken-wrong, CI-catches-it-eventually wrong. Same feature, ten bespoke wirings, eight quiet mistakes.

That's not a tidiness problem, that's a correctness problem. Copy-paste isn't just harder to maintain; it's a machine for manufacturing bugs that all look slightly different.

So the shared pieces moved into [one library](https://github.com/gi-os/BrightCommon), published to GitHub Packages, and every app pulls it in as a dependency like anything else.

## Small design notes from a small library

A couple of decisions I'd defend:

**One call, no partial states.** `LightReport.install(...)` sets up reporting *and* arms the crash handler. There's no way to end up with an app where shake-to-report works but crash capture doesn't, because there's no second call to forget.

**Inert by default.** Skip the install call entirely and the whole feature is dead weight — the overlay renders nothing, nothing is queued. An app can adopt the library without adopting every feature in it.

**The overlay is a sibling, not a wrapper.** It draws in its own window, so it doesn't care what layout it's called from, and adopting it doesn't mean restructuring your UI tree.

One honest annoyance for anyone tempted to do the same: GitHub Packages requires authentication even for public packages. There's no anonymous read. Every consumer needs a token with `read:packages`, which for a hobby ecosystem is pure friction with no upside I can find.

None of this is novel. Shared libraries are the oldest idea in software, and "don't repeat yourself" is on the first page of every book. But there's a difference between knowing the advice and watching eight hand-ports go subtly wrong in one afternoon. Now a wheel fix is one edit and a version bump, and the apps stopped drifting apart. Boring work. Recommend it.
