---
title: "Installing OpenWrt on a TP-Link TL-WA801ND v3"
date: 2022-09-14T16:54:00
tags: ["Guides", "Hardware", "Infrastructure", "Linux", "Networks", "Software"]
---

Over the past few years I've been looking at getting as much 'open hardware' as possible. Knowing something's running an open source operating system puts me at ease versus something that's just a black box where you have no idea what's actually going on.

Virgin Media's 'super hubs' are quite imfamous for wireless coverage so a separate AP was pretty much mandatory as my router is in my cellar and the furthest distance from it is more than 3 floors away making coverage tricky.
I picked up a TP-Link TL-WA801N access point pretty much when I moved into this house some 8 years ago and while it's far from up to date, or well specced, for a simple access point not running much of anything else it will more than suffice.

Recently however I've been finding that it will kind of crash, or freeze requiring a reboot to get it working again. With this, I fancied a bit of a change and the best looking/documented and supported alternative operating system seemed to be [OpenWrt](https://openwrt.org/).


The installation instructions are pretty vague, but you can stumble your way through easily enough. I tried to install from the web interface as mentioned in the guide but had no luck so fell back to the TFTP server method.

First things first, you'll need the two binary installation files, [available here](https://openwrt.org/toh/tp-link/tl-wa801nd). The 'install' `*-squashfs-factory.bin` will need renaming to `wa801ndv3_tp_recovery.bin` before you start. The method for installing the OS is detailed in the [Locked Firmware](https://openwrt.org/toh/tp-link/tl-wa801nd#locked_firmware) of the guide.

* Configure your computer on `192.168.0.86`, offering `wa801ndv3_tp_recovery.bin` over TFTP.
* I used [tftpd32](https://bitbucket.org/phjounin/tftpd64/downloads/) for this, but any server should work.
* Reboot your AP with the reset button held down to enter recovery mode.
* You'll see the recovery bin file be transferred and the AP will reboot as `192.168.1.1`.
* Access this via SSH, to ensure you can login as `root` and you're greeted with the OpenWrt MOTD.
* Log out, then copy the `*-sysupgrade.bin` package over using scp: `scp openwrt-18.06.9-ar71xx-tiny-tl-wa801nd-v3-squashfs-sysupgrade.bin root@openwrt.lan:/tmp`
* Log back into the AP using SSH, and verify the file is there as the correct size using `ls /tmp/*.bin`
* If this is correct, flash the image using `sysupgrade -v /tmp/openwrt-18.06.9-ar71xx-tiny-tl-wa801nd-v3-squashfs-sysupgrade.bin`

What I ended up needing to do now (as I needed Internet access for package management), was to set up the AP as a DHCP client using the following:
```
uci set network.lan.proto=dhcp"
uci commit network
/etc/init.d/network restart
```
The MAC address my AP used when requesting an IP  was different to the one I'd previously assigned it while using the stock firmware (A7 vs A6 as the last character) which meant my pre-assigned lease was incorrect and needed amending in the DHCP reservations in my router.

With this done however you should be able to log back in using the IP given via DHCP, and then update your package list using `opkg update`. With this done, you can finally install a Web Interface using `opkg install luci`.

You can now access this newly installed Web Interface and make any other changes you require. In my case, I just needed to edit and then enable the Wireless SSID under Network > Wireless.

With that complete, enjoy your new open source access point!