---
title: "Concatenating Videos on Windows with FFmpeg"
date: 2019-05-09T09:21:00
tags: ["Formats", "Guides", "Media", "Snippets", "Software", "Windows"]
---

I recently had a bit of a binge on the Japanese comedy show Gaki No Tsukai. This show is basically impossible to find officially so we're heading to streaming sites (as linked on their [subreddit](https://www.reddit.com/r/GakiNoTsukai/)) to get copies of the show. As these shows are usually multiple hours in length they're going to be split into multiple parts.

As expected, these files can be grabbed via `youtube-dl`, and then manually renamed into an easy to work with format (01.mp4, 02.mp4, etc.) as files with spaces can be problematic in my experience.

Naturally, you'll need ffmpeg installed and ready to use. Windows builds can be grabbed from [this site](https://ffmpeg.zeranoe.com).

Once you have these video files, cd into the working directory and run the following to make a list of files that ffmpeg can interpret.
```
(for %i in (*.mp4) do @echo file '%i') > concat.txt
```
Ensure the order is correct as you definitely don't want these out of order.

Now you can run the following:
```
ffmpeg -f concat -i concat.txt -c copy concat.mp4
```

Now you should have a single `concat.mp4` file that plays through as you'd expect it to be.