---
title: "Case-sensitively labelling a FAT32 drive"
date: 2020-07-05T17:11:00
tags: ["Linux", "Snippets", "Software", "Windows"]
---

## Under Linux

As you'd expect, it's pretty simple. You just need `fatlabel` from the `dosfstools` package:

```
sudo apt-get install dosfstools
lsblk --fs
fatlabel /dev/yourdrive YourLabel
```

## Under Windows

Windows generally won't let you do this, always defaulting to upper-case, but there is a workaround. Create a new text file on the root of the drive called `autorun.inf` with the following contents.

```
[autorun]
label=YourLabel

[Content]
MusicFiles=false
PictureFiles=false
VideoFiles=false
```
