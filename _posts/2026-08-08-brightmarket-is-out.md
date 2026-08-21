---
layout: post
title: "BrightMarket is out"
date: 2026-08-08 12:45:00 -0400
tags: ["Light Phone","BrightMarket"]
description: "The app store ships: browse on a desktop, install by QR, track updates on the phone, and get your own app into the catalog."
---

BrightMarket shipped yesterday. It's two halves that meet in the middle.

**The browsing lives on the desktop.** [brightmarket.gzl.dev](https://brightmarket.gzl.dev) is a real catalog — read about a tool, decide if you want it, scan its code with the phone, and it opens in BrightMarket ready to install. Browsing on a big screen, installing on the small one.

**The tracking lives on the phone.** The other half watches GitHub releases and tells you when something's out of date. It'll track any repo that publishes APKs, including ones that have nothing to do with the catalog, so you give up nothing you already follow. And you don't rebuild your setup by hand: import your Obtainium export and it takes the lot. Catalog apps become normal entries. Everything else is tracked exactly as it was.

## The choice at first launch

The part I sweated most: a gallery on your phone is a scrollable feed, and a scrollable feed on a Light Phone kind of defeats the point. So the app asks, first thing. Keep browsing on the phone if you want it, or go extra light, and the phone only ever shows what you've installed and what needs updating. New apps arrive by scanning a code off the desktop. You can still patch something at 2 a.m. without finding a laptop. You just can't wander.

The friction is asymmetric on purpose. Turning browsing on is one tap. Turning it back off means scanning a code from the desktop page. Falling off the wagon should be slightly harder than climbing back on. I wrote about [why the app is built this way](/blog/an-app-store-for-a-dumbphone/) separately.

## What's in it

At launch: the twenty-odd apps I've built for my own phone plus my forks of community tools — a camera, an ebook reader, RSS and newsletter readers, a TOTP authenticator, live scores, subway times, an Apple TV remote, a notebook, a couple of games. All free, all open source.

## Getting your own app in

If you've made something for the LPIII, add it. Open [the submit page](https://brightmarket.gzl.dev/submit.html), sign in with GitHub, pick a repo of yours that has a release, done. The sign-in asks for `read:user` and nothing else: it can see which public repos you own and can't write anywhere.

Everything after that is a script. It checks that the release has an APK attached, reads the applicationId out of the APK itself rather than trusting what the form told it, and opens a pull request. Once that lands, the next index build picks your app up and it shows in the catalog.

Two things worth knowing before you submit. The repo has to publish a real release with the APK attached, not just source. And the applicationId is what the update tracker keys on, so changing it later breaks the update path for everyone who already installed you.

## Launch day, honestly

The best bug reports arrived within hours. Uninstalling wasn't obvious enough — fixed. Adding a repo required making a QR code when typing a URL would do — added. This is the whole reason to ship: a week of real users beats a month of me squinting at my own UI. If Light eventually ships an official tools store, I genuinely hope it's better than mine. Until then: [brightmarket.gzl.dev/apk](https://brightmarket.gzl.dev/apk) on the phone, and everything else installs from inside.
