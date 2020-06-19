---
title: "Manually formatting, mounting and using Hetzner volumes"
date: 2019-07-04T15:40:00
tags: ["formats", "linux", "servers", "snippets", "software"]
---

I've recently moved all my server infrastructure over to Hetzner, and to date everything's been going swimmingly.

The default partition options though aren't ideal, so I'm scrapping my existing volume and recreating it manually, properly.

Firstly, login to your Hetzner account and create your volume:

```
Volumes > Create Volume > Size in GB
Name whatever
Mount options Manual
Create & Buy Now
```

Now find the partition after logging into your server via issuing `lsblk`. In my case, this was `/dev/sdb`

You can now partition this new drive as a GPT volume by doing the following:
```
gdisk /dev/sdb
o
n
enter
enter
enter
w
enter
```

One partitioned, you can format this new partition as ext4, via the following:
```
sudo mkfs.ext4 /dev/sdb1
```
Seeing as though I'm going to be using this partition as extra storage for downloaded files I don't really need the reserved blocks it offers, which can be disabled via:
```
sudo tune2fs -m0 /dev/sdb1
```

Now, we'll make a mount point for the newly formatted volume:
```
mkdir $HOME/mountpoint
```

It's also worthwhile grabbing the disk's UUID via `sudo blkid /dev/sdb1 -s UUID -o value`.

Now we're going to add an entry in `/etc/fstab` so the partition will be mounted automatically. You'll need to edit this file as root and add the following line:
```
UUID=your-uuid-from-above /home/youruser/mountpoint ext4 discard,nofail,defaults 0 0
```

Now that your `fstab` file is ammended, you can remount all your partitions via: `sudo mount -a`

Last but not least, change the owner of the directory to prevent file permission issues:
```
sudo chown youruser:youruser /home/user/mountpoint -R
```
