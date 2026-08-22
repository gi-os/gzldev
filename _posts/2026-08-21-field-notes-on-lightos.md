---
layout: post
title: "Field notes on LightOS"
date: 2026-08-21 11:48:00 -0400
tags: ["Light Phone", "LightOS", "Android"]
description: "What the Light Phone III's OS actually is underneath: Android with the Google removed, hardware that talks in key events, and conventions nobody voted on."
---

After a few dozen apps and a lot of evenings with `adb`, I've accumulated notes on what LightOS — the Light Phone III's operating system — actually is under the surface. None of this is secret. All of it was learned the slow way, one confused evening at a time, and it's scattered across my repos in README paragraphs that assume you already care. This is the collected version, so the next person doesn't have to spend the evenings I did.

## It's Android 14, and that's a feature

LightOS is Android 14 underneath, with a locked bootloader. No jailbreak, no unlock, no exotic toolchain — `adb install` works the way it works everywhere, and ordinary modern APKs run. The entire community app scene exists because Light left this door open, and I choose to believe it's open on purpose.

What's *removed* matters more than what's changed, and the removals fail in a very particular way: **the API stays, and answers nothing.** There are no Google services, and Android's plumbing quietly assumes there will be. Some examples I've personally lost time to:

- `android.speech.SpeechRecognizer` exists, but there's no provider to bind to. `android.speech.tts` exists, but no engine is installed. A voice assistant on this phone has to send both directions to the cloud — that's why [LightVoice](https://github.com/gi-os/LightVoice) is built around Whisper and a TTS API rather than anything on-device.
- `SpellCheckerSession` accepts your requests and corrections silently never arrive, because nothing is installed to answer them. [LightKeyboard](https://github.com/gi-os/LightKeyboard) bundles its own 63k-word dictionary and a bigram table because that's the only spell-check that will ever exist here.
- There's no push messaging. Anything that needs to be told something has to bring its own transport, which is why my iMessage client talks to a self-hosted server over Tailscale.

The pattern is worth internalizing before you build anything: on LightOS, a dead API doesn't error. It just never calls you back.

## The hardware talks in key events

This is the finding that unlocked the most, so it gets the most space. The wheel, the camera button, and the home button all arrive at whichever app has focus as ordinary `KeyEvent`s. Light patched `/system/usr/keylayout/Generic.kl` — the keylayout every input device on the phone loads — and you can read the whole story in five lines:

```
key 19    WHEEL_CCW      # wheel up       (Pixart pat9126ja, was R)
key 20    WHEEL_CW       # wheel down     (Pixart pat9126ja, was T)
key 66    WHEEL_CLICK    # wheel press    (gpio-keys, was F8)
key 80    FOCUS          # camera stage 1 (was NUMPAD_2)
key 27    CAMERA         # camera stage 2 (was RIGHT_BRACKET)
```

Things I now know that I wish someone had told me:

- **The wheel is a mouse part, not a rotary encoder.** It's a Pixart pat9126ja optical sensor firing one discrete DOWN+UP key pair per notch, roughly 35–60 ms apart. Android's actual rotary plumbing — `AXIS_SCROLL`, `onRotaryScrollEvent` — never sees a thing. Anything wheel-aware has to be built on raw key events, counting notches yourself.
- **`WHEEL_CCW`, `WHEEL_CW` and `WHEEL_CLICK` aren't AOSP keycodes.** They're Light's labels, and you resolve them by name at runtime. Hardcode the numeric codes and you're betting against a firmware update.
- **The camera button is two-stage**, like a real shutter: half-press sends `FOCUS`, full press sends `CAMERA`. Two keys, two gestures, and the stock camera's behavior stops being magic.
- **Nothing intercepts any of this in `PhoneWindowManager`.** The brightness ramp and flashlight in Light's own tools are app-layer code in those tools. In your sideloaded app the keys arrive and nothing listens, which feels like a missing API and is actually an invitation.

The only sanctioned way to hear these keys *globally* — without having window focus — is an `AccessibilityService` with `flagRequestFilterKeyEvents`; the injection route (`INJECT_EVENTS`) is signature-only and closed to you. That's what [BrightControl](https://github.com/gi-os/BrightControl) is. One design note I'd urge on anyone building similar: declare `canRetrieveWindowContent="false"` and subscribe to the minimum event types you can. An accessibility service asks for real trust, and the manifest is where you make the limits checkable rather than promised.

## Notifications are boringly standard, and that's a gift

LightOS posts fully standard Android notifications, so `NotificationListenerService` just works — that's the entire basis of [LightGlance](https://github.com/gi-os/LightGlance), which draws ambient notification dots on the black panel. No parsing of Light's internals, no package allowlist, just the platform doing what the documentation says.

The catch is granting the permission: LightOS has no Settings screen for notification access, so the grant is adb-only —

```
adb shell cmd notification allow_listener <component>
```

— and this generalizes. **A recurring LightOS pattern is that the capability exists but its Settings UI doesn't**, so runtime permissions that would normally show a dialog get granted with `pm grant`, appops with `appops set`. The grants survive reboots. Every one of my READMEs with a permissions section is really documenting this same gap.

While I'm here: there is no reachable always-on display. A real AOD is a low-power panel self-refresh mode owned by SystemUI and the panel driver, and a sideloaded app cannot touch it on any Android. If you see "AOD" on a sideloaded LightOS app, it's the screen genuinely on at minimum brightness, and the honest budget for that is 2–4%/hr. Glance's default is to wake for eight seconds when something arrives, because that costs approximately nothing and does most of the job.

## Networking has one big footgun

Join a Wi-Fi network that has no internet — a camera's access point, say — and Android keeps routing your traffic over LTE. Every socket you open goes to the wrong network, and it presents as a device that pairs happily and then refuses to talk. The fix is `WifiNetworkSpecifier` to join plus `bindProcessToNetwork` to pin your sockets, and as a bonus the specifier path means the system shows the network picker, your app never sees a scan result, and you don't need the location permission every other camera app demands. [BrightImport](https://github.com/gi-os/BrightImport) is built on exactly this, and its CI fails the build if a location permission ever sneaks into the manifest.

## Bluetooth: BLE yes, L2CAP no

Standard BLE scanning works fine — [LightPods](https://github.com/gi-os/LightPods) reads AirPods battery levels straight out of Apple's proximity advertisements with nothing but `BLUETOOTH_SCAN`. But the richer channel (AAP over L2CAP, where noise control and real ear detection live) is walled off three ways: Android didn't allow third-party L2CAP sockets on this codebase until a fix that landed in Android 16, the LP3 is Android 14, and the bootloader is locked so the usual workaround can't install. Knowing which wall you're behind saves you from debugging the wrong one.

## The panel sets the rules

Everything renders grayscale, and designing for it is closer to print than to normal app work. Contrast does the work color would. Patterns replace palettes — when I needed tape labels to be distinguishable in the recorder, the answer was geometry, not hue. Type carries almost everything, and LightOS's own type and spacing conventions are consistent enough that matching them makes an app feel native with no other effort.

## The conventions nobody voted on

The community converged on packaging norms the way small scenes do — quietly, by copying each other. One APK per GitHub release. Tags ending in a number. A stable signing key per app so upgrades install in place; I go one further and pin the certificate's SHA-256 in the repo with CI failing on drift, because a changed certificate otherwise surfaces only as `Failure: Invalid` on someone's phone. My update tracking and the app index lean on all of these, and none of them are written down anywhere official. They're just what works, repeated until they became load-bearing.

That's the phone as I currently understand it. Nothing here is a discovery — it's all sitting in system files, AOSP sources, and other people's repos. But small platforms live or die on whether the folklore gets written down, and this is me writing mine down.
