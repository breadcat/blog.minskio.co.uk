---
title: "Finding and removing fake subtitles"
date: 2020-09-16T13:38:00
tags: [ "Guides", "Linux", "Media", "Movies", "Servers", "Snippets", "Software" ]
---

A recent trend that I absolutely hate is the inclusion of fake *advertisement* subtitles in pirated video releases. As a user and a fan of subtitles, this just presents me with extra steps when I actually want the correct subtitles for the media, I need to go about finding the original release name, then find the correct subtitle file before replacing the fake file. All in all, it's a hassle I'd prefer not to have. Perhaps this is my fault for downloading horrible remux rips from horrible places in the first place.

A complete fake subtitle file will read a little something like this:
```
1
00:00:30,000 --> 00:00:36,000
Provided by YTS.MX

2
00:00:36,500 --> 00:00:42,000
Find the official YIFY movies site at
https://YTS.MX

3
00:30:30,000 --> 00:30:36,000
Downloaded from YTS.MX

4
01:00:30,000 --> 01:00:36,000
Download more movies for free
from YTS.MX
```

Anyway, we can find these and similar files using the following command:
```
find . -type f -iname "*.srt" -size -4096c -exec grep YTS -l {} \;
```

This will search the current directory for any files that end in `.srt`, are below 4KB in size, and specifically contain the text string `YTS` which is the main offender.

With these results you can manually check the files and delete them if you'd like.