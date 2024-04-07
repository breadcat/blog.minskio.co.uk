---
title: Logging Duolingo ranks over time
layout: single
date: 2022-05-03T14:58:00
lastmod: 2024-04-07T12:10:00
---

I was curious as to how my rank would change over time and what the rate of attrition would look like over time.

This page was originally an attempt to mix markdown and bash scripting into a single executable page that updates itself when run that was itself documentation of how it worked, however the header that Hugo (which is what this site is built with) requires entirely breaks the markdown/bash quirk that allowed it to work with plain markdown.

In the background, the data source I'm scraping is [duome.eu](https://duome.eu/) and dumps values directly to this page using `blog_duolingo_rank` as part of `server.sh` in [my Dockerfiles project](https://github.com/breadcat/Dockerfiles).

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
| 2022-05-07 | 23:12 | 2909 | 3252 |
| 2022-05-10 | 10:05 | 3271 | 3271 |
| 2022-05-16 | 09:57 | 3299 | 3299 |
| 2022-05-19 | 17:15 | 2818 | 3244 |
| 2022-05-28 | 23:33 | 2793 | 3213 |
| 2022-05-30 | 19:58 | 3222 | 3222 |
| 2022-06-13 | 12:26 | 2742 | 3201 |
| 2022-06-28 | 12:40 | 3106 | 3318 |
| 2022-07-12 | 18:25 | 3320 | 3320 |
| 2022-08-03 | 13:19 | 3155 | 3310 |
| 2022-08-04 | 09:07 | 3152 | 3311 |
| 2022-09-28 | 12:13 | 3059 | 3207 |
| 2022-10-17 | 12:12 | 3152 | 3152 |
| 2022-11-29 | 22:37 | 2966 | 3085 |
| 2023-02-28 | 18:23 | 2922 | 3001 |
| 2023-05-23 | 23:38 | 3030 | 2958 |
| 2023-09-12 | 00:00 | 2985 | 2985 |
| 2023-10-11 | 13:47 | 2842 | 2842 |
| 2023-11-13 | 20:32 | 2819 | 2819 |
| 2023-11-29 | 23:23 | 2843 | 2843 |
| 2023-12-28 | 11:33 | 2863 | 2863 |
| 2023-12-31 | 23:59 | 2866 | 2866 |
| 2024-01-07 | 23:59 | 2869 | 2869 |
| 2024-01-14 | 23:59 | 2873 | 2873 |
| 2024-01-21 | 23:59 | 2875 | 2875 |
| 2024-01-28 | 23:59 | 2881 | 2881 |
| 2024-02-04 | 23:59 | 2884 | 2884 |
| 2024-02-08 | 09:49 | 2885 | 2885 |
| 2024-02-11 | 23:59 | 2887 | 2887 |
| 2024-02-18 | 23:59 | 2892 | 2892 |
| 2024-02-25 | 23:59 | 2895 | 2895 |
| 2024-03-03 | 23:59 | 2897 | 2897 |
| 2024-03-04 | 01:54 | 2897 | 2897 |
| 2024-03-10 | 23:59 | 2900 | 2900 |
| 2024-03-17 | 23:59 | 2903 | 2903 |
| 2024-03-24 | 23:59 | 2903 | 2903 |
| 2024-03-31 | 23:59 | 2906 | 2906 |
| 2024-04-07 | 12:10 | 2913 | 2913 |
