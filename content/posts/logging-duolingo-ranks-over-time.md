---
title: Logging Duolingo ranks over time
layout: single
date: 2022-05-03T14:58:00
lastmod: 2022-05-04T14:43:00
---

I was curious as to how my rank would change over time and what the rate of attrition would look like over time.

This page was originally an attempt to mix markdown and bash scripting into a single executable page that updates itself when run that was itself documentation of how it worked, however the header that Hugo (which is what this site is built with) requires entirely breaks the markdown/bash quirk that allowed it to work with plain markdown.

In the background, the data source I'm scraping is [duome.eu](https://duome.eu/) and dumps values directly to this page using `func_duorank` as part of `server.sh` in [my Dockerfiles project](https://github.com/breadcat/Dockerfiles).

Currently this script is manually run whenever I'm curious. Perhaps as time goes on, I'll automate this and eventually graph out these values (similarly to my [weight tracking](/weight/) page), but that's a project for later.


| Date   | Time | Streak | Lingots |
| ---------- | ----- | ---- | ---- |
| 2022-04-10 | N/A   | 3078 | 3337 |
| 2022-04-11 | N/A   | 3068 | 3343 |
| 2022-04-12 | 11:39 | 3056 | 3347 |
| 2022-04-14 | 23:35 | 3047 | 3355 |
| 2022-04-19 | 23:57 | 3314 | 3314 |
| 2022-04-20 | 11:37 | 2986 | 3321 |
| 2022-04-24 | 23:06 | 2962 | 3336 |
| 2022-05-03 | 15:17 | 2909 | 3316 |
| 2022-05-04 | 14:43 | 2910 | 3316 |
