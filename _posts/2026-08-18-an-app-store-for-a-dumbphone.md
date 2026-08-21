---
layout: post
title: "Why I put an app store on a phone designed to have none"
date: 2026-08-18 21:58:00 -0400
tags: ["Light Phone", "BrightMarket"]
description: "Yes, I see the irony. The argument for building it, why there's no server behind it, and the afternoon I accidentally dropped everyone's apps from the index."
---

I am aware of how this sounds. The Light Phone III is a phone built on the idea that you should stop looking at your phone. I built an app store for it.

Let me at least explain how I got here. [The launch post](/blog/brightmarket-is-out/) covers what shipped and how to get your own app listed. This is the other half: why it exists at all, what's holding it up, and the afternoon I broke it.

## The problem was never installing apps

Sideloading works fine on this phone. LightOS is Android underneath, and an APK installs the way an APK installs anywhere. The problem is what happens three weeks later, when you have fifteen sideloaded apps and every one of them updates on its own schedule, on its own GitHub release page, and nothing on the phone knows any of that.

[Obtainium](https://github.com/ImranR98/Obtainium) solves this, and solves it well. I used it for months. But it's built for a normal Android phone, and it only knows about the repos you paste into it yourself. There was no answer to the more basic question: how does anyone find out these apps exist at all? For a while the honest answer was "a Reddit thread and word of mouth."

So BrightMarket does the same update tracking Obtainium does, fits this phone, and adds a catalog on top. Find an app, install it, get told when it needs updating. That's the whole product.

## The uncomfortable part

An app store on a minimalist phone is still an infinite scrolling feed. I couldn't design my way out of that, so I stopped trying and made browsing optional instead. On first launch the app asks. With Focus mode on, it shows what you have installed and what needs updating, and nothing else. No catalog, no feed, no discovery. A surprising number of people pick that mode, which I choose to read as the userbase keeping me honest.

## No server, on principle and also on budget

There is no backend. The "database" is [a public GitHub repo](https://github.com/gi-os/brightmarket-index): one YAML file per app, and a CI job that rebuilds a single JSON index every hour and serves it from GitHub Pages. Submissions arrive as pull requests against that repo, which means the whole catalog has a public history — you can read every app that was ever added, by whom, and when.

Download counts come from GitHub's own release statistics, so the "most downloaded" sort costs nothing and tracks nobody. The app never phones home. I couldn't run analytics if I wanted to, which is the correct amount of temptation to have.

## The part where I got it wrong

One afternoon in August, the GitHub Releases API started returning empty lists. Not errors — clean HTTP 200 responses with nothing in them. My builder read "no releases" and helpfully dropped those apps from the index. Other people's apps, gone from the catalog, because of my code and someone else's outage.

An HTTP error had always kept the previous entry. A *successful empty response* didn't, because I'd never imagined one. The rule that came out of it is now written into the repo: an app is never dropped for an answer the API failed to give.

Somewhere along the way this thing became how most Light Phone III owners install community apps. I didn't plan for that and I try not to think about it too hard. Mostly it means the bar for "good enough" moved, and the empty-response bug is what taught me where it moved to.
