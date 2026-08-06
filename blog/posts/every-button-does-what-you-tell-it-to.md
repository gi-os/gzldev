A few weeks ago I wrote about [reading the keylayout file](/blog/the-keylayout-file-light-left-behind/) and learning that the Light Phone's wheel and buttons are just key events that most apps ignore. That post was the archaeology. This one is about the app that came out of it, [BrightControl](https://github.com/gi-os/BrightControl), and the design questions that turned out to be harder than the code.

## The pitch is one sentence

The wheel, the camera button, and the home button work inside every app, not just Light's. That's it. Turn the wheel anywhere and you get brightness — or scrolling, or nothing, your choice, per app. Tap the wheel for the flashlight. Hold it for anything you want. Every gesture on the phone is a slot you can bind.

## Defaults are the actual product

The code for "catch a key, do a thing" is an afternoon. Deciding what the *right* thing is took much longer, and I changed my mind a lot.

The rule I landed on: out of the box, BrightControl should feel like Light finished the job rather than like a third party moved in. So the defaults copy Light's own behavior — wheel is brightness, tap is flashlight, camera button opens the camera — and they extend it to everywhere it didn't reach. On LightOS's own screens, the wheel stays LightOS's entirely, unless you deliberately switch that off. I didn't want to be the app that broke the phone's own settings page.

The one invention I'll claim is small: double-tap the wheel to switch it between brightness and scrolling, and the phone tells you which one you're now holding. On a screen this minimal, a mode you can't see is a mode you'll get wrong, so the mode announces itself.

## The trust problem

There's no polite way around this part: BrightControl works because it's an accessibility service that can see key presses in every app. That is a real amount of trust to ask for, and "trust me" is not an argument. The honest mitigations are the boring ones — the source is public, the app has no network permission to send anything anywhere, and the README explains exactly which mechanism it uses and why the sanctioned API for this job happens to be the accessibility one. If you read that and still don't want it on your phone, that's a reasonable decision and the phone works fine without it.

## What I've learned from the bindings

People bind the strangest things, and the reports that come in have quietly become a survey of what this phone is missing. The hold-gestures especially — an empty slot that can launch anything turns out to be a suggestion box. More than one app in my collection started as somebody's binding.
