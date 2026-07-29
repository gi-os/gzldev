The modern world hands you addresses as QR codes — menus, tickets, Wi-Fi, the poster at the bus stop — and the Light Phone III shipped unable to read a single one of them. The stock camera doesn't scan. There's no browser worth typing into even if you could bear typing a URL on a 3.9-inch keyboard. So the codes just sit there, smug.

[LightQR](https://github.com/gi-os/LightQR) is two small tools that meet in the middle:

**The scanner** is an Android app. CameraX supplies frames, and ZXing decodes them off the luminance plane — a detail that matters here, because ZXing is pure Java. No Google vision services, no Play dependency, which on LightOS isn't a preference but a requirement: there is nothing else to call. Scan a code, read the text, open the link if it is one. Every scan lands in a local history. No cloud, no account, nothing leaves the phone.

**The generator** is the desktop twin: a static page at [gi-os.github.io/LightQR](https://gi-os.github.io/LightQR/), served straight from the repo's `docs/` folder by GitHub Pages. Type anything on a machine with a real keyboard, get a code, scan it with the phone. It sounds almost too dumb to build. It's become the single most-used piece of plumbing in my whole setup, because "get a long string onto a phone with a tiny keyboard" turns out to be a constant of dumbphone life — my voice assistant's setup literally depends on it for API keys.

There's no server anywhere in this. The scanner talks to nothing; the generator is a file. I keep rediscovering that the same architecture — a plain APK plus, at most, a static page — covers a remarkable share of what this phone is missing, and every part of it keeps working even if I get hit by the B62.
