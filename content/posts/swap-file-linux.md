---
title: "Creating a Swap File on Linux"
date: 2019-06-10T10:03:00
tags: ["formats", "guides", "linux", "servers", "snippets", "software"]
---

I've recently moved from a server with more than enough RAM, to a lower spec (and significantly cheaper!) VPS that still does 99% of what I want it to.

The issue however is that with the reduced RAM there's a very real possibility of running out and locking up the system. The easy (and cheap) solution is to add a swap file instead of repartitioning my disk space.

The notes here work with a 1GB swap file, but feel free to change these if need be.

Firstly we're going to create the file, set permissions and enable the swap file:
```
sudo fallocate -l 1G /swap
sudo chmod 600 /swap
sudo mkswap /swap
sudo swapon /swap
```

This can then be mounted on boot by editing your `/etc/fstab` file and adding the following line:
```
/swap swap swap defaults 0 0
```

Now you can check this swap space is working by:
```
sudo swapon --show
```

Now the swap file is up and running, you can decide if you want to alter the *[swappiness](https://en.wikipedia.org/wiki/Paging#Swappiness)* value on your system which dictates how the file should be used. The default value on my system was `60`, but I've altered it to `10` using the below commands:
```
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
```
To make this change permanent, you'll need to write the value to your `sysctl.conf` file via:
```
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
```
