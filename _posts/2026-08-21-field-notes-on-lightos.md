---
layout: post
title: "Field notes on LightOS"
date: 2026-08-21 11:48:00 -0400
tags: ["Light Phone", "LightOS", "Android"]
description: "What the Light Phone III's OS actually is underneath: Android with the Google removed, hardware that talks in key events, and conventions nobody voted on."
---

After a few dozen apps and a lot of evenings with `adb`, I've accumulated notes on what LightOS — the Light Phone III's operating system — actually is under the surface. None of this is secret and most of it is probably known to everyone who's poked at the phone. But it was scattered across my repos and my head, and the next person shouldn't have to spend the evenings I did.

## It's Android, and that's a feature

LightOS is Android underneath — a real one, recent enough that ordinary modern APKs install and run. There's no jailbreak, no unlock, no exotic toolchain. `adb install` works the way it works everywhere. The entire community app scene exists because Light left this door open, and I choose to believe the door is open on purpose.

What's *removed* matters more than what's changed: there are no Google services. No Play Store, and no Google push messaging. Any app that assumes Firebase is standing behind it falls over quietly. That constraint shaped my whole approach — anything that needs a push has to bring its own transport, which is why my iMessage client talks to a self-hosted server over a private Tailscale network instead of waiting for a notification system that isn't there.

## The hardware talks in key events

The wheel, the camera button, the home button — all of them arrive as ordinary Android key events, remapped in a plain-text keylayout file on the system partition. The wheel is literally a mouse sensor part. I wrote this up properly in [the keylayout post](/blog/the-keylayout-file-light-left-behind/), but the headline is: there's no private hardware API. Light's own apps listen for the same key codes yours can. The only sanctioned way to hear them *globally* is an accessibility service, which is what BrightControl is.

## The panel sets the rules

Everything renders greyscale, and designing for it is closer to print than to normal app work. Contrast does the work color would. Patterns replace palettes — when I needed tape labels to be distinguishable in the recorder, the answer was geometry, not hue. Type carries almost everything, and LightOS's own type and spacing conventions are consistent enough that matching them makes an app feel native with no other effort.

## The conventions nobody voted on

The community converged on packaging norms the way small scenes do — quietly, by copying each other. One APK per GitHub release, tags ending in a number, a stable signing key per app so upgrades install in place. My update tracking and the app index lean on all of these, and none of them are written down anywhere official. They're just what works, repeated until they became load-bearing.

That's the phone in four notes. Nothing here is a discovery. But small platforms live or die on whether the folklore gets written down, and this is me writing mine down.
