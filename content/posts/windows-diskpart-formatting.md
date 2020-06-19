---
title: "Windows Diskpart Formatting"
date: 2018-11-13T15:02:00
tags: ["guides", "snippets", "software", "windows"]
---
If you've flashed any live-bootable Linux ISO files or similar to a USB drive you may notice that when trying to format again to the full size it seems capped to the original size of the ISO file.

A rough guide for resolve this how to do so is below:

```
win + r
diskpart
list disk
select disk 1
clean
create partition primary
format fs=ntfs quick label=Keyring
assign letter=Z
exit
```
