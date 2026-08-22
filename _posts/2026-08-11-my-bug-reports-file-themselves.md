---
layout: post
title: "My bug reports file themselves"
date: 2026-08-11 18:47:00 -0400
tags: ["Light Phone", "AI", "tooling"]
description: "Shake the phone three times and the app files its own GitHub issue. Then an AI triages the queue every hour — under strict instructions that guessing is worse than waiting."
---

Every app I ship for the Light Phone has the same hidden feature: shake the phone three times and it files a bug report. Not an email, not a form — a GitHub issue, opened by the app itself, with the screen you were on, a table of build and device info, the last crash log if there was one, and a screenshot. If the app crashed outright, it asks on the next launch whether to send what it caught.

I built this because the alternative was worse. Bug reports for phone apps normally arrive as a Reddit comment three weeks later: "the camera thing broke." Which screen? Which version? Gone. The reporter has moved on, and honestly, fair — describing a bug is work, and nobody owes me work. So the app does the describing. The person just shakes.

## The 360-pixel witness

The screenshots come through grayscale and 360 pixels wide, and I expected them to be useless. They're the most valuable part of the report. A broken layout, a stuck spinner, a date that's obviously wrong — you can see it instantly, in a way no prose description gets close to. Most reports are diagnosable from the screenshot and the build table alone.

## Then I stopped being the bottleneck

The reports land in one repo, and for a while "triage the queue" was a thing I did with coffee. Now an AI agent does the first pass, every hour. It reads each new report, clones the app's source fresh, and works out what happened.

The rules I gave it are mostly rules about restraint:

- **Fix it** only when the cause is *identified* — a stack trace naming the line, code that visibly does the wrong thing — and the fix is small and contained. A null guard. An off-by-one. A missing state reset.
- **Hand it to me** when the cause is a guess, when the fix would change behavior someone might have an opinion about, when it needs the physical phone, or when it touches more than about three files.
- Never reuse a stale checkout, because pushing from an old base silently deletes whatever landed in between, and it compiles perfectly afterward.
- Branch first, always. In these repos a push to main cuts a signed APK that's on people's phones within minutes, with no human between a mistake and the device. CI runs the same checks on a branch and publishes nothing.

The line I keep coming back to when tuning it: guessing is worse than waiting. A wrong fix ships. A waiting issue just waits.

## Does it work?

For the boring majority of reports — yes, and the boring majority is the point. Null guards and layout constraints get diagnosed, fixed, tested, and released while I'm at work, and the release notes say what changed. The interesting bugs, the ones where the report is really a design question wearing a trench coat, get labeled and handed to me with the agent's notes attached. That split is the whole value: I only see the reports that actually need a human, and the person who shook their phone gets a fix in hours instead of whenever I next had a free evening.

The plumbing that makes this possible in every app at once — the shake detector, the report overlay, the crash handler — lives in a shared library, which is its own unglamorous story for another post.
