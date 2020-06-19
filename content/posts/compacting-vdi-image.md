---
title: "Compacting a VirtualBox VDI Image"
date: 2018-09-06T12:26:00
tags: ["formats", "guides", "linux", "snippets", "software", "windows"]
---

Firstly, you'll need to install `zerofree` on your Linux image, then mount the root partition as a read only volume. This can be done via:

```
sudo apt-get install zerofree
sudo apt-get clean
sudo reboot
# interrupt grub boot and boot into recovery mode
mount -n -o remount,ro -t ext4 /dev/sda1 /
zerofree /dev/sda1
shutdown -h now
```

And now on the Windows Host:
```
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" modifyvdi "VirtualBox VMs\server\server.vdi" compact
```

This should make the VirtualBox image more in line with the calculated size it should be.