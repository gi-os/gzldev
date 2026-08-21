---
layout: post
title: "Rewriting the camera"
date: 2026-08-02 20:10:00 -0400
tags: ["Light Phone", "design"]
description: "Roll's whole design is one gesture, stolen from a contact sheet. Plus the filters that got away from me — purikura on a monochrome phone, and datamoshing experiments that mostly produce mud."
---

A camera app is not a new idea. I want to say that before anything else, because [Roll](https://github.com/gi-os/Roll) is the app people bring up most, and the truth is that most of it is things every camera app has: a shutter, filters, a gallery. The only part I'd defend as mine is the layout, and even that is stolen — from paper.

## One gesture

Here's the whole design. The photo roll sits *above* the viewfinder. Pull down on the camera and your photographs come into view — the newest one against the corner of the frame, older ones running up and leftward behind it, the way a contact sheet reads. Flick up from anywhere and you're back at the shutter.

That's it. That's the entire navigation model. There are no tabs, no gallery icon in a corner, no separate app to open. The camera and everything it has ever produced are one continuous surface, and you move between them the way you'd tilt a stack of prints toward yourself.

I spent longer on this than on any feature. Not building it — deciding it. Every draft with more structure felt like the phone it was replacing. The Light Phone's whole argument is that a screen this size, in greyscale, can't sustain interfaces built from chrome and menus, so the interface had to be a gesture you could learn in one use and never think about again.

## Why a rewrite and not a fork

The stock Camera and stock Album are two separate apps, and the boundary between them is exactly the thing I wanted to remove. There was no way to fork my way there; the design decision I cared about lived in the space between the two codebases. So Roll replaces both, and it's written from scratch. You can set it as the phone's default camera, so the hardware camera button opens it.

The wheel works as a lens ring, because a physical dial on the side of a camera is too good a coincidence to waste.

## The filters got away from me

I planned three filters. There are now more than I'm comfortable counting, and the filter picker needed its own scrolling design to hold them. In my defense, a greyscale sensor is a strange and generous canvas — with no color to protect, you can push tone curves, grain, and halftone dots much further before a photo stops feeling like a photo. Film looks with stamped date backs. Dithering that turns a shot into something a 1985 Mac would print. A Bulge lens for when a cat's face demands it. None of this is novel — film simulation is a whole industry and I'm at the hobbyist end — but the constraint makes it fresh to work in.

The one I'm most attached to is the purikura filter. If you've ever crammed into a Japanese photo-booth, you know the look: blown-out skin, inky outlines, everything slightly a sticker. I spent a summer in Tokyo in high school and a shameful amount of it in those booths, so this one is less a filter than a memory with a tone curve. It has no business being on a monochrome dumbphone. People use it constantly.

The current experiments are with datamoshing — deliberately corrupting the compression so motion smears and blocks bleed into each other. It's mostly a video-codec trick, which makes it awkward to do to stills, so I've been faking the spirit of it: shifting blocks between consecutive frames from the film-roll burst and letting the errors accumulate. Most attempts produce mud. Every so often one produces a photograph that looks like a memory degrading in real time, which is exactly the energy this phone deserves, and why I keep going. Nothing has shipped yet. Some of it may never — a filter that fails nine times in ten is a darkroom, not a feature.

## What shipped it

Roll has shipped more releases than anything else I've made, and I can't take credit for the pace. People keep sending good ideas. Every one of my apps has shake-to-report built in, and a decent fraction of what comes through isn't bug reports at all — it's *what if pulling down went behind the viewfinder instead of over it*, from someone I've never met, who was right.
