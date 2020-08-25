---
title: "Calculating relative average subdirectory filesizes"
date: 2020-08-21T12:37:00
tags: [ "Guides", "Linux", "Media", "Snippets", "Software" ]
---

During these _unprecedented times_ I've been watching a fair amount of movies and TV shows, and deleting once done, as you do. As a bit of a interesting insight and guiding hand I've been using the excellent [ncdu](https://dev.yorhel.nl/ncdu) and [rclone's ...clone ](https://rclone.org/commands/rclone_ncdu/) which both work excellently for when 1 folder equates to 1 _media_. With TV shows however this is complicated somewhat. In steps calculating average filesizes in a directory so you can sort them revealing the most notorious offenders.

I had a quick look online but couldn't find anything that really did what I wanted so I wrote it myself in an extremely verbose fashion. Eventually I slimmed it down to the following function:
```
for i in *; do s=$(du -s "$i" | awk '{print $1}') && c=$(find "$i" -type f -size +1M | wc -l) && echo "$((s / c)) $c $s $i"; done | sort -nr
```

What this does is:
- Loops every item in the folder using `for`
- Calculates the size `$s` using `du`
- Counts the items in the folder `$c` using `find` ignoring small files, like subtitles
- Divides the two to get the average
- Prints the list, then reverse sorts it

Giving you your results. Ta~da!