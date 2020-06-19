---
title: "Resetting Windows Passwords from Linux with chntpw"
date: 2018-12-13T09:58:00
tags: ["Guides", "Linux", "Snippets", "Software", "Windows"]
---

I came across a strange issue recently, where I needed to reset a users password however the computer was unable to boot from USB so my usual [ntpasswd](https://pogostick.net/~pnh/ntpasswd/) option was out of the question.

Step in the ever userful `chntpw` which is available in the Debian repositories and can be installed via:
```
sudo apt-get install chntpw ntfs-3g
```

`ntfs-3g` is also thrown into the mix so you can mount the inevitable Windows NTFS partition with write permissions

From here on, we'll be executing everything as root to avoid strange permission issues, so elevate yourself via `su`.

Find your disks Windows partition via `blkid` and mount it via:
```
mkdir /mnt/win && ntfs-3g /dev/sdb2 /mnt/win
```
Where `sdb2` is your Windows Partition.

Then `cd` to your System32's config directory with:
```
cd /mnt/win/Windows/System32/config
```
and ensure the `SAM` registry hive is located there with `ls`.

Now start the software on the SAM file with `chntpw -i SAM`. Follow the instructions onscreen to blank the password and unlock the account if need be. When quitting ensure you save your changes then boot the Windows drive and enjoy your new unlocked Administrator (or other) account.