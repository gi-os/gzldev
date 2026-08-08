BrightMarket shipped yesterday. I used Obtainium for months and it works fine — but it never felt like it belonged on this phone. Too complex, too many features aimed at a device this isn't. And it only solved half the problem: it kept apps updated, but there was still nowhere to *find* them. The honest answer to "how do I discover Light Phone apps" was a Reddit thread.

So BrightMarket is two halves that meet in the middle:

**The browsing lives on the desktop.** [brightmarket.gzl.dev](https://brightmarket.gzl.dev) is a real catalogue — read about a tool, decide if you want it, scan its code with the phone, and it opens in BrightMarket ready to install. Browsing on a big screen, installing on the small one.

**The tracking lives on the phone.** The other half replaces Obtainium: it watches GitHub releases and tells you when something's out of date. It'll track any repo that publishes APKs — including ones that have nothing to do with the catalogue — so you give up nothing you already follow. And you don't rebuild your setup by hand: import your Obtainium export and it takes the lot. Catalogue apps become normal entries; everything else is tracked exactly as it was.

## The choice at first launch

The part I sweated most: a gallery on your phone is a scrollable feed, and a scrollable feed on a Light Phone kind of defeats the point. So the app asks, first thing. Keep browsing on the phone if you want it — or go extra light, and the phone only ever shows what you've installed and what needs updating. New apps arrive by scanning a code off the desktop. You can still patch something at 2 a.m. without finding a laptop; you just can't wander.

The friction is asymmetric on purpose. Turning browsing on is one tap. Turning it back off means scanning a code from the desktop page. Falling off the wagon should be slightly harder than climbing back on.

## What's in it, and how to get in

At launch: the twenty-odd apps I've built for my own phone plus my forks of community tools — a camera, an ebook reader, RSS and newsletter readers, a TOTP authenticator, live scores, subway times, an Apple TV remote, a notebook, a couple of games. All free, all open source.

If you've made something for the LPIII, add it: sign in with GitHub, pick a repo of yours that has a release, done. The sign-in asks for `read:user` and nothing else — it can see which public repos you own and can't write anywhere.

## Launch day, honestly

The best bug reports arrived within hours. Uninstalling wasn't obvious enough — fixed. Adding a repo required making a QR code when typing a URL would do — added. This is the whole reason to ship: a week of real users beats a month of me squinting at my own UI. If Light eventually ships an official tools store, I genuinely hope it's better than mine. Until then: [brightmarket.gzl.dev/apk](https://brightmarket.gzl.dev/apk) on the phone, and everything else installs from inside.