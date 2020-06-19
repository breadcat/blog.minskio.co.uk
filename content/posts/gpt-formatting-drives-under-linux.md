---
title: "GPT formatting new drives under Linux"
date: 2018-11-14T10:42:00
tags: ["guides", "linux", "servers", "snippets"]
---

So, you've bought a shiny new hard drive and would like to format it and use it on your headless Linux server.
For this guide, we'll be partitioning it as a GPT partition to support sizes over 2TB, and formatting it as ext4.

Find device id (/dev/sdc, etc):
```
lsblk
```

Edit the drive, using fdisk in GPT mode with:
```
sudo gdisk /dev/sdc
d
n
[enter] x5
w
```

Make a partition:
```
sudo mkfs.ext4 /dev/sdc1
```

Verify everything formatted correctly:
```
lsblk --fs /dev/sdc1
```

Find the drives new UUID:
```
blkid /dev/sdc1
```

Lastly, add a mountpoint to `/etc/fstab`:
```
UUID=00000000-0000-0000-0000-000000000000 /mnt/mount-point  ext4 defaults 0 0
```
