---
title: "ext4 deleted data recovery"
date: 2022-05-09T17:07:00
tags: ["Linux", "Recovery", "Snippets", "Software"]
---

Two data recovery stories in as many weeks! Fortunately, the events that lead to these were actually split by well over a year.

Inevitably, while using Linux you're going to end up making the human error of deleting a file you shouldn't delete. I was working in two near-identical file trees slowly merging them together. As you've guessed, I `rm -rf`'d a directory in the wrong tree. I immediately knew as soon as I hit enter so unmounted the drive and started researching recovery methods.

As I was still working on the directories, inevitably I hadn't made a backup.

I was lucky for a number of reasons:
* I still had access to a working live system
* the drive was mostly full
* it was just one directory
* I had an available drive to start dumping recovered files to immediately.
* the drive hadn't been written to since the deletion


After installing (but not using) photorec and testdisk I wanted to see if there was anything else that could work. I looked up `extundelete` which sounded perfect, but wouldn't compile on my system.

In steps `ext4magic`. After looking at a long winded guide which included dumping your journal I had a flick through the documentation and settled on the following command:

```
umount /dev/sdb1
sudo ext4magic /dev/sdb1 -m -d /mnt/rescue
```

What the above will attempt to do is search `/dev/sdb1` (recovering deleted file with the `-m` flag) and copy anything found to destination `/mnt/rescue`.

I can't accurately guage how successful the process was but I certainly recovered files that had been deleted.
