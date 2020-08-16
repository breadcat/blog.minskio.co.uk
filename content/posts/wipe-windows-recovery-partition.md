---
title: "Wiping your Windows recovery partition"
date: 2020-08-14T03:42:00
tags: [ "Guides", "Snippets", "Software", "Windows" ]
---

I recently replaced my Windows 10 LTSC installation with a stock(er) Windows 10 Pro install, one thing I noticed was an additional partition automatically being created. This partition was helpfully labelled as a recovery partition, and took up 512MB of ever so precious space. I did a bit of research into the matter and found out it wasn't strictly necessary, and personally when it comes to recovering an operating system I have no qualms performing a clean install.

So, to remove this, first launch an admin command prompt (search for `cmd` in your start menu, right click, run as administrator) and run the following:

```
diskpart
list disk
select disk 0
list partition
select partition 4
delete partition override
```
*in the example above, our recovery partition is #4*

You can now expand your system partition into the empty space.

Launch `diskmgmt.msc`, right click your system partition, then extend volume.
