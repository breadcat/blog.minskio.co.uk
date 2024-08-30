---
title: "My rudimentary backup process"
date: 2024-07-26T09:56:00
tags: ["Guides", "Infrastructure", "Linux", "Servers"]
---

I've heard of the 3-2-1 backup process, however I can't easily, securely (or cheaply) keep an offsite backup so I opt for more of a 1-2 process. I keep an offline store that's manually synced up every month or so.

# The problem

 As you can imagine though, there are a few problems with this... the moving parts involved are:
 
* [NAS](/hardware/) - Contains my live data, but can only accept 1 hard drive (hardware fault), can't be hot-swapped, doesn't have a screen or keyboard connected and only has USB2 speed ports for data transfer.
* [Desktop](/hardware/) - Connected via gigabit LAN, and has spare SATA ports for connection, however runs Windows.
* [Laptop](/hardware/) - Ancient Thinkpad with USB3, running Linux however connects to the network using Wi-Fi and USB-SATA adapters are always a bit unreliable.
* Backup hard drive - An offline 12TB SATA drive with the current backup.

# The Solution

So, with the above outlined, I feel as though the solution I came up with was quite elegant. We're going to use rsync on the NAS to send changes to the backup drive connected to our main Workstation computer (temporarily) running Linux from a live environment. Phew.

## Destination

First [boot the Arch Linux ISO](/whats-on-my-usb-key/) on your destination machine with the destination disk attached to it. I have to hammer `F11` on my machine but it varies.

Once fully booted, check the environment you're in, mount your drive and set up a password for SSH access:

```
lsblk --fs
mount /dev/sdb1 /mnt
passwd
whoami
ip a
```

With the above details confirmed, it's time to begin the real process.

## Sending

Using the laptop, ensure you're able to connect to both the NAS, and then from the NAS, connect the desktop.
```
ssh nas
ssh root@192.168.1.4
```

If you've already run the process previously, SSH verification will fail and you'll need to remove the keys from your `.ssh/known_hosts` file either manually, or via `ssh-keygen -R 192.168.1.4`.

Now we can send our files, on the NAS, run the following (inside a `tmux` session if you think it's going to take a while):

```
rsync -avhP --delete --exclude=lost+found/ /mnt/ root@192.168.1.4:/mnt
```

Once complete, you've got an up to date backup and can shut down your live environment and disconnect the hard drive for another time.

# Error handling

Once, after a _very_ unscheduled power outage, my next backup failed with a data integrity error. I assumed this was the destination drive throwing the error as I'd had no errors reported on the source, however this appeared to be a silent failure. Only one file was affected and I was able to redownload the affected file without too much hassle. My process went a little something like...

Stopping the NFS server and unmount the volume:
```
sudo systemctl stop nfs-server.service
sudo umount /mnt
```

Using `fsck` on the volume, fixing any errors it finds, then re-mounting and restarting our NFS server:
```
sudo fsck.ext4 /dev/sdb1 -fy
sudo mount -a
sudo systemctl start nfs-server.service
```
