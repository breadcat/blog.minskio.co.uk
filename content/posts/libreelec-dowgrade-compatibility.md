---
title: "Downgrading LibreElec, skipping compatibility checks"
date: 2018-12-19T09:35:00
tags: ["HTPC", "Kodi", "Linux", "Snippets", "Software"]
---

I run Libreelec on my ODROID-C2 SBC as a HTPC. The combination work surprisingly well and I rarely have any issues. Perhaps stupidly however, I updated from the stable 8.2.5 to the latest beta build to try out the RetroPlayer functionality. Long story short, this didn't pan out so well and I ended up in *SAFE MODE* with not a whole lot working.

I tried downgrading using addons but this also gave me some grief with compatibility checks. The error I was receiving was because I was using an `arm` compile of the software, but I was trying to install `aarch64` which is perfectly compatible with the C2 board but not according to the compatibility check.

So, to manually downgrade, ssh into your board while in safe mode.

```
ssh root@libreelec
```
Your default root password will intuitevely be `libreelec`.

Then navigate to your upgrade directory:
```
cd /storage/.update/
```

Grab the latest stable version of the software:
```
wget http://releases.libreelec.tv/LibreELEC-Odroid_C2.aarch64-8.2.5.img.gz
```

And disable the compatibility check on upgrade via:
```
touch .nocompat
```

Lastly, reboot, and enjoy your hopefully stable LibreElec installation.
```
reboot
```

I noticed watched status and a few of my settings had defaulted but aside from this everything worked perfectly. Rejoice!