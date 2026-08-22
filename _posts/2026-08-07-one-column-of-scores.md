---
layout: post
title: "One column of scores"
date: 2026-08-07 18:40:00 -0400
tags: ["Light Phone","BrightSports"]
description: "Twenty-two leagues from four keyless public APIs, in one grayscale column. How BrightSports gets scores with no account and no push."
---

I follow too many teams for a phone with no scores app. LightOS has no sports surface at all, and refreshing ESPN's mobile site on a 3.9-inch grayscale screen is a punishment I kept choosing anyway. So: [BrightSports](https://github.com/gi-os/BrightSports). One column of scores, grouped by day. Standings, one tab per league. A game page with first pitch, venue, TV listing, and both records. A notification when something happens to a team you follow. That's the whole app, and the restraint is the feature — on this screen, a second column is a design failure.

## Twenty-two leagues, zero API keys

The part I'm actually pleased with is the data story. The app covers MLB, NFL, NBA, NHL, MLS, five European soccer leagues plus the two UEFA cups, FBS college football, four women's leagues, four levels of minor league baseball, and Formula 1 — and there is no account, no API key, and nothing to paste in, ever. Four public JSON providers carry all of it:

**ESPN's site API** does the heavy lifting: every major, the European soccer, college football, most of the women's leagues, and F1. The lovely thing about it is that the scoreboard response shape is identical across team sports, so one parser covers everything. The unlovely thing is standings, which nest differently per league — the fix is to walk the response's `children` tree recursively instead of indexing into it and hoping.

**MLB StatsAPI** covers the four minor league levels, selected by `sportId` (11 through 14 for Triple-A down to Single-A). ESPN publishes no minor-league scoreboard at all; this is the same feed MiLB.com itself runs on, which is as close to the source as you can stand.

**HockeyTech/LeagueStat** carries the PWHL, and the fourth provider fills the last gaps. College football needed one extra trick: ESPN's feed returns roughly 750 schools, so the app filters it down to the ~140 FBS programs, because nobody following Alabama wants to scroll past every Division III scoreline to find them.

## Boring on purpose

Under the hood this is the least clever app I ship, deliberately. Follows are a `SharedPreferences` string set. Caches are plain files. There's no database, no Room, no annotation processing in the build at all — clone it and it compiles fast and dumb. After a year of building increasingly elaborate plumbing for the other apps, there was real pleasure in writing one where the complete persistence layer is "a set of strings."

Notifications work the way everything works on a phone with no push services: the app checks the feeds itself on a schedule and posts ordinary local notifications when a followed team's game changes state. No server of mine in the loop, nothing tracking what you follow. If the phone is off, you find out the score the way people did in 2004, which frankly suits this phone.
