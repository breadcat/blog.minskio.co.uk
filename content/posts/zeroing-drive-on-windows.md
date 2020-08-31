---
title: "Zeroing out a drive on Windows"
date: 2020-08-28T20:34:00
tags: ["Guides", "Servers", "Snippets", "Software", "Windows"]
---

Recently I sold a couple more of the hard drives that made up my SnapRAID array back when I had a Linux powered home server. Wanting to check over the drives one last time for SMART info, and bad sectors I noticed I hadn't wiped the drive, and there were still readable files and partitions there.

Previously, I did this all via a USB3 dock on my Linux laptop using `dd`, but today we're on a Windows desktop, so let the adventure commence!

The command we want to run, in an elevated command prompt is:
```
format g: /fs:NTFS /p:1
```
In this example, we're formatting drive **G**. The `/p` parameter here is what's doing the zeroing, indicating the number of passes.

Now to **fully** clean a drive, after the above you can wipe the partition table from the disk by running:
```
start > run > cmd
diskpart
list disk
select disk 1
clean
```

And you're done.