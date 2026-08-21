---
layout: post
title: "The MetroCard inside my phone"
date: 2026-08-06 21:35:00 -0400
tags: ["Light Phone","hardware","NFC"]
description: "I dissolved a transit card in acetone and moved its nervous system into the battery compartment. Tap-to-pay is back, sort of."
---

Of everything I gave up moving to this phone, tap-to-pay was the one I actually felt. Not apps, not the camera — the turnstile. Standing at the OMNY reader fishing for a card while the person behind me radiates New York patience. The Light Phone III has NFC hardware in it, and the community keeps asking when it'll do something; so far, nothing shipped uses it. So I stopped waiting and put a MetroCard inside the phone.

I posted the short version [on Reddit](https://old.reddit.com/r/LightPhone/comments/1vgb3iv/how_to_add_transit_cards_to_your_lightphone/). This is the long one.

## A card is packaging

The thing to understand about a contactless card is that almost none of it is the card. Inside the plastic there are exactly two functional parts: a chip about the size of a grain of rice, and a loop of wire running around the card's perimeter. The wire is the antenna, and it's also the power supply — the card has no battery. The reader radiates a 13.56 MHz field; the loop harvests enough energy from it to wake the chip, and the same coupling carries the conversation. That's the whole machine. The plastic is packaging, and packaging can go.

## The acetone part

Transit cards are laminated PVC. Soak one in 100% acetone overnight — pure hardware-store acetone, not nail polish remover, which has oils in it — and the laminate gives up. The layers soften and peel, and what's left is the chip with its antenna coil attached, looking like the card's nervous system with the body dissolved away.

![An OMNY card dissolving in a bowl of acetone, layers separating, the mag stripe peeling away](/assets/blog/metrocard-acetone.jpg)
*Overnight in the bowl. The laminate gives up in layers; the mag stripe goes first.*

Handle it like a cobweb. The coil is fine wire, the bond between coil and chip is the weakest point, and there is no fixing it if it tears. It took me a few tries to end up with a working one. I don't think the task is genuinely hard; I think I'm clumsy, and the difference matters because you should budget more than one card either way.

## The install

The chip-and-coil goes in the battery compartment. Two things I learned at actual turnstiles, by failing at them:

The coil wants to be spread to the very edges of the cavity while staying a coil. Loop area is everything — the voltage the chip harvests scales with the area the field passes through, and a scrunched-up coil is a deaf one.

Orientation matters more than it has any right to. Chip black-side down reads most reliably for me. I can't give you the physics for that one; I can give you the turnstile data.

![The Light Phone III's open battery compartment with the chip and antenna coil held under strips of blue painter's tape, the fine wire spread to the compartment's edges](/assets/blog/metrocard-installed.jpg)
*An earlier build, taped in for testing — you can see the coil spread to the cavity's edges. The one currently in my phone is working too consistently to risk opening up for a photo.*

## The part where the phone fights back

First full test: intermittent failures, maybe one read in three. The culprit is the phone itself. The LP3's NFC front end is a coil too, sitting in the same few cubic centimeters, and even idle it detunes the space around it — two resonant loops that close together pull each other's resonance off frequency, and a transit chip living on harvested power has no margin for that.

The fix is to shut the phone's radio up:

```
adb shell svc nfc disable
```

and `svc nfc enable` to undo it. The setting survives reboots. With the phone's NFC off, my read rate went to effectively always. I'm working on an app to make this toggle less of a ritual, because a hardware mod that requires `adb` for daily operation is only half done.

## Living with it

This is the part that makes it practical rather than a stunt: OMNY lets you register a card on their website and attach auto-reload. So the card inside my phone refills itself, and the phone taps through turnstiles like it always should have. My card expires in 2033. When it does, I'll do it again.

The obvious next question — someone asked it within an hour — is a credit card. It should work identically; it's the same physics and mostly the same chips. I haven't tried, for the boring reason that I can't walk into my bank and ask for five test cards. If you try it, I want to hear how it went. And for the advanced class: one commenter re-wound a card's antenna around the bare chip with a NanoVNA to verify the resonant frequency, and wears it as a ring. That's real radio work. Mine is the lazy version, and I'm at peace with that.

## Caveats, honestly

Do this to a card you can afford to kill, because your first one probably dies. Acetone wants ventilation and patience. The MTA would presumably rather you didn't, so consider this documentation, not advice. And if the phone ever ships a real wallet, I'll be first in line to dissolve nothing at all.
