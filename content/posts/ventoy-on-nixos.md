---
title: "Installing Ventoy from NixOS"
date: 2026-09-14T12:51:00
tags: ["Linux", "Servers", "Snippets"]
---

A while ago I needed to format my USB key, but after this needed to install Ventoy back on it.

Previously I was using Windows as my primary desktop OS, but I'm still slowly (very slowly) moving everything over to NixOS.

So, without further ado; here's how I installed Ventoy on a USB stick from NixOS. This guide assumes you're using bash, if not; you can enter a bash shell with `bash`.

The Ventoy package currently is [considered insecure](https://github.com/NixOS/nixpkgs/issues/404663) on NixOS so you'll need to allow the installation of insecure packages before you begin, but then it's just a case of installing it via nix-shell.
```
export NIXPKGS_ALLOW_INSECURE=1
nix-shell -p ventoy
```

With that installed, you can now check over your drives, and install on `sdc` (replacing this with your USB key device):
```
lsblk --fs
sudo ventoy -i /dev/sdc
```

After this, check that it's correctly installed with `sudo ventory -l /dev/sdc`

Now, you can add your [images](/whats-on-my-usb-key/) to make this newly installed USB key actually useful:
```
lsblk --fs
sudo mount /dev/sdc1 /mnt/usb
sudo mv *.iso /mnt/usb/ -vin
sudo umount /mnt/usb
```
