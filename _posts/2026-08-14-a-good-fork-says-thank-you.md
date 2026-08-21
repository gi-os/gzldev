---
layout: post
title: "A good fork says thank you"
date: 2026-08-14 15:21:00 -0400
tags: ["Light Phone", "open source"]
description: "A lot of what I ship is other people's work with my name nearer the top than it deserves. Some rules I try to fork by."
---

A decent amount of what I ship for the Light Phone is other people's work with my name nearer the top than it deserves. BrightChat is [Craig Eley's chat app](https://github.com/craigeley/chat). BrightMusic builds on Jonathan Caudill's phono. BrightNews started as zachattack323's LightRSS. BrightLibrary is a FastRead fork, and BrightThumb is a port of Thumb-Key. None of these were my ideas. The community around this phone is tiny, the good ideas are unevenly distributed, and forking is how the work compounds.

Which is fine — that's what open source is for. But forking has an etiquette, I've been on both ends of it now, and I have opinions.

## Keep their README

When I forked Craig's chat app, his README didn't get rewritten — it got moved to the bottom of mine, preserved as he wrote it, under his name. The top of the file says plainly: Craig wrote the app; here's what this fork adds; nothing here is upstream's responsibility. That last part matters more than the credit. The worst thing a fork can do to its upstream isn't stealing glory, it's leaking bug reports. If my Favorites tab breaks, that issue belongs on my repo, and the README's job is to make sure it lands there and not in Craig's inbox.

## Never touch the applicationId

This one's technical and absolute. On Android, the applicationId is the app's identity. Change it and you haven't renamed the app — you've created a second app that installs alongside the first, and every user's data stays trapped in the old one. Several of my apps still carry `light*` package names from before a rename, and they always will. The name on the screen is free to change. The identity underneath is not, and no amount of tidiness justifies breaking it. This rule is written into the BrightMarket index with a comment that says, roughly, do not "fix" this.

## Renaming is not rebranding

The Bright\* names exist so the collection reads as a collection — so someone who liked one app can find the others, and so update tracking has a consistent home. The rename is packaging, not authorship. If the name change ever starts implying the work is mine when it isn't, the README exists to un-imply it, loudly and first.

## Send things back when they fit

Not everything I add belongs upstream — a Favorites tab tuned to this phone's wheel probably doesn't help a general-purpose app. But when a fix is general, it should travel. The fork relationship shouldn't be a one-way valve.

I don't think any of this is original wisdom either. It's mostly what the licenses already ask, plus some manners. But small communities run on the assumption that building on someone's work is a compliment and not a raid, and that assumption survives exactly as long as forks keep saying where they came from.
