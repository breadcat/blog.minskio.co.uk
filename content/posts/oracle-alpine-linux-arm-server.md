---
title: "Oracle Alpine Linux ARM server"
date: 2022-02-16T18:59:00
lastmod: 2022-03-08T12:03:00
tags: ["Guides", "Linux", "Networks", "Servers", "Software"]
---

After finding a post about [creating a minecraft server in bash](https://sdomi.pl/weblog/15-witchcraft-minecraft-server-in-bash/) on Reddit I started reading through the rest of [sdomi's blog](https://sdomi.pl/weblog/) and [one post in particular](https://sdomi.pl/weblog/12-bootstrapping-alpine-on-oraclecloud/) caught my eye. It details running an aarch64 version of Alpine Linux on a free tier Oracle Cloud server. Now this combination of words ticks pretty much every box I have going. While the guide is a little brief on details it works great and offers some suggestions at the end I'd like to build upon. So without further ado, let's go.

## Initial Instance

After creating your account, create your compute instance. While creating this, change the following under 'Image and shape':
* Change the Image to 'Oracle Linux version 7.9' (correct at time of writing)
* Change the Shape to be 'Ampere, Arm-based processor'

I uploaded my own public key, but you can also download pre-generated ones. All the other defaults are fine. Click create and wait the five or so minutes for the instance to be created.

Once created, log in via the 'Public IP address' on the instance details page, using your SSH private key with the username `opc`. With this, you should be logged in and you can elevate to root using `sudo su -`.

Now [follow the rest of the guide](https://sdomi.pl/weblog/12-bootstrapping-alpine-on-oraclecloud/) and reboot into your newly booted Alpine VM.

## Upgrading to Edge

The first thing I did once rebooted was upgrading from version `3.13` of Alpine to the latest `edge` version. This is entirely optional, but I prefer to run the latest versions of applications where possible.
To do this, edit your repositories file:
```
vi /etc/apk/repositories
```
Replace any references to `v3.13` with `edge`, then run the following to update:
```
apk --update upgrade
```
Reboot again, and you should be running the latest version. Feel free to add any additional pacakges using `apk --update add neofetch neovim` etc.

## Shuffling Partitions

As mentioned in the guide, Alpine is currently installed on the /old/ swap `/dev/sda2` partition. You can now delete your old Oracle partiton to regain some 40GB space.

```
fdisk /dev/sda
p
d
3
w
```
Which should (remember to read the printed output!) delete the old XFS Oracle partition.

While we're here, you can also change the partition type on `/dev/sda2` from Linux Swap to Linux Filesystem
```
fdisk /dev/sda
t
2
linux
w
```

You can now write your changes with `w` and quit with `q`.

For the next stage, we need to expand the size of our Alpine ext4 partition.
```
apk add parted
parted /dev/sda
p
resizepart
2
yes
100%
quit
```

With this size now increased, you can uninstall parted with `apk del parted` and actually go about resizing it, using the following command `resize2fs /dev/sda2`.
Once completed, reboot again and check your partitions using `fdisk -l`.

## Securing SSH

As mentioned in the guide, all the above remote work was done using password authentication which we're not a fan of in the long term. I did this from my *other* server for easy access to my SSH keys:
```
ssh-copy-id -i your_ssh_key.pub root@server.ip.address
```
Verify your `.ssh/authorized_keys` file looks correct, ensure you can connect using SSH and public key authentication. If this works, you can now disable password authentication with the following:
```
nvim /etc/ssh/sshd_config
```
Search for the `#PasswordAuthentication yes` line, uncomment it and change this to `no`. Here you can also change the port SSH is active on, predictably with the `Port 22` line, be sure to check the Firewall section however if you do change this.

Once you're done, save the file and restart the SSH daemon using `rc-service sshd restart`.

Congratulations, you've now (reasonably) secured your SSH server. You can also go one step above with this and disallow root logins, and create a new non-root user, but that's outside the scope of this guide.

## Swap
One last thing that needs attention is that during the installation of Alpine we used the 8GB swap partition as our root. Without going into re-partitioning we can simply create a new swap file of whatever size is required (in this example, I'm using 2GB). To do this, simply do the following:
```
dd if=/dev/zero of=/swap bs=1M count=2048
chmod 0600 /swap
mkswap /swap
swapon /swap
```
You can now check swap status using `free -h`. To automatically mount this swap file on boot, add the following line at the end of your `/etc/fstab` file:
```
/swap none swap sw 0 0
```

## Firewall
Again, as mentioned in the excellent guide, all ports except tcp/22 are blocked by default. If you'd like these opening, in your Oracle account go to Networking > Virtual Cloud Networks > vcn-creationdate-time > Security Lists > Default Security List > Ingress Rules.
Here, delete the 3 existing rules (if you'd like to respond to ICMP packets), then create a new rule with the following details:

* Stateless: No
* Source Type: CIDR
* Source CIDR: 0.0.0.0/0
* IP Protocol: All Protocols

Once saved, you should be able to access anything you're hosting as you'd expect.

## Changing Shells
Lastly, I quite like the fish shell (hate all you want), but the `chsh` utility is missing so to change your shell you need to manually edit your `/etc/passwd` file, replacing `/bin/ash` with `/usr/bin/fish` or whatever other shell you'd like to use.

## Supporting Ansible
I'm looking at managing my servers with ansible, the requirements here are somewhat simple, all you need is a python binary which can be installed via:
```
apk add python3
```

## Block Storage
Much like [Hetzner](/manually-formatting-mounting-and-using-hetzner-volumes/), Oracle also offer Block Storage with the added bonus of being free. I decided to opt for the 4 OCPU/24GB instance and then use my remaining 150GB block storage creating a storage volume. When creating this there are a few things to note:
* Ensure the availability domain is the same between instance and block storage
* When you're attaching the volume to the instance, use *paravirtualized* instead of ISCSI

Now, you can check your storage is available by issuing `lsblk`, your disk should be listed as `/dev/sdb`. To start using this storage you can do the following:
```
fdisk /dev/sdb
n
{enter}
{enter}
{enter}
{enter}
w
```
This should give us a new partition to use. We can now format it to ext4 with:
```
mkfs.ext4 /dev/sdb1
```
We can also reduce the reserved space on this partiton giving us a little more breathing room:
```
tune2fs -m1 /dev/sdb1
```
All that's left now is to edit `/etc/fstab` and add an entry like the following:
```
/dev/sdb1       /mnt/tank       ext4    rw,nofail	0	0
```
You can now mount everything using `mount -a`, and you're done.

* **Edit 2022-03-08:** Added block storage instructions