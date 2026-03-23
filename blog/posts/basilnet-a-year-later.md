A year ago I wrote up [BasilNet](/blog/building-basilnet/), my Docker home server stack, with the confidence of someone whose setup had been stable for almost three weeks. Time to report back on what survived.

## What broke, and what it taught me

The best bug of the year: qBittorrent and the arr apps run inside Gluetun's network namespace, so their traffic can't leak around the VPN. Great for privacy. It also means they don't live on my LAN — and one day I needed one of them to talk to Plex, which runs in host network mode, and nothing worked. The containers couldn't reach the server's own LAN address because, from inside the VPN namespace, that address effectively doesn't exist.

The answer took an evening I won't get back: go through the Docker bridge gateway. From inside the namespace, `172.18.0.1` is the way home. It's obvious in retrospect, which is true of every networking problem I have ever had. I wrote it down in the server's own docs so future me doesn't get to enjoy discovering it twice.

Also in the spirit of honest reporting: I switched VPN providers along the way for boring price reasons, and the migration was one env block in the Gluetun config. That part of the design earned its keep.

## What I'd do differently

Fewer services. The stack at its peak ran more than fifteen containers, and the honest usage graph says maybe eight earn their memory. There's a homelab failure mode where you start running software because you *can*, and the maintenance quietly becomes a second job with no salary. I've been slowly trimming toward the set that gets used weekly: Plex, the arr apps behind the VPN, the request manager the family actually touches, Home Assistant.

Overseerr remains the surprise winner of the whole project. Nobody in my life wants to talk about Docker, but everybody can ask for a movie.

## The job it grew into

The thing I didn't predict: BasilNet became infrastructure for my other projects. Every Light Phone app I ship can back itself up, and those backups land here — one agent, one container, one file per app. The server that started as "I want a media server" is now the quiet destination for a phone's worth of notes and photos.

That's the actual lesson of the year. A home server's value isn't the stack you install in the first excited week. It's having a machine of your own, always on, that every future project gets to assume exists.
