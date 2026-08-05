I want to be clear up front: I didn't discover anything here. Everything in this post was sitting in a plain text file on the phone, readable by anyone with `adb` and some patience. I just seem to be the one who wrote it down.

## The mystery

The Light Phone III has a scroll wheel, a camera button, and a home button. In Light's own apps they do lovely things — the wheel ramps brightness, tap it and the flashlight comes on. Install your own app and all three buttons go dead. Nothing. For a while I assumed there was some private API, some system service Light's apps talked to that mine couldn't.

There isn't. It's much simpler and much stranger.

## What's actually in there

Android maps hardware keys through keylayout files, and Light patched the generic one — `/system/usr/keylayout/Generic.kl`, the layout every input device on the phone loads. Pull it off the phone and there it is:

```
key 19    WHEEL_CCW      # wheel up       (Pixart pat9126ja, was R)
key 20    WHEEL_CW       # wheel down     (Pixart pat9126ja, was T)
key 66    WHEEL_CLICK    # wheel press    (gpio-keys, was F8)
key 80    FOCUS          # camera stage 1
key 27    CAMERA         # camera stage 2
```

The wheel is a Pixart optical sensor — the same family of part that lives in a mouse — and its rotations arrive at whichever app has focus as ordinary `KeyEvent`s. Keys that used to be R and T, remapped. That's it. That's the whole system.

Which means the brightness ramp in Light's tools isn't the OS doing something privileged. It's app-layer code inside those tools, listening for key 19 and key 20 like any app could. Nothing intercepts these keys in the window manager. In a sideloaded app, they arrive, and nothing is listening.

## Building the missing layer

Once you know that, the fix suggests itself: something needs to listen everywhere. You can't inject events on Android without a signature-level permission, but an `AccessibilityService` with `flagRequestFilterKeyEvents` can see keys for windows it doesn't own — it's the one sanctioned way to do this, and it's the same mechanism a voice assistant uses for its push-to-talk key.

So that's what [BrightControl](https://github.com/gi-os/BrightControl) is. A service that catches the wheel, the camera button, and the home button in every app, and gives each one behavior: brightness or per-notch scrolling on the wheel (double-tap to switch, and it tells you which), flashlight on a tap, anything you like on a hold. Per-app overrides, so LightOS's own screens keep LightOS's behavior unless you say otherwise.

None of this is clever engineering. It's one accessibility service and a switch statement. The work was the archaeology — pulling files off the phone, reading them, and testing what listens to what. The README carries the full writeup, because it's the documentation I spent an evening wishing existed.
