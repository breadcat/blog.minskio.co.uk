---
title: "Upgrading (and Downgrading) Skulls on a Thinkpad laptop"
date: 2020-11-16T22:59:00
tags: ["Guides", "Hardware", "Linux", "Software"]
---

After posting [my previous guide](/flashing-skulls-to-a-thinkpad-x230t/) regarding [Skulls](https://github.com/merge/skulls) I had been running version 0.1.8 on my laptop without issue. Never wanting to leave something good alone though, I decided to endeavor to upgrade the BIOS image to the latest version, because why the hell not.

The first thing you'll want is to install the two dependencies the script requires.
```
sudo pacman -S dimdecode flashrom
```

Now grab the [latest release](https://github.com/merge/skulls/releases) from the projects Github repository and extract the archive using `tar -xf skulls-x230t-0.0.2.tar.xz`.

Now, reboot your computer and interrupt grub using your arrow keys before pressing `e` to edit your boot options.

You want to replace the `quiet` parameter on the `linux` line with `iomem=relaxed` then press F10 to boot with these new options.

Once booted, navigate back to your extracted skulls directory and run the shell script that will internally flash Skulls:
```
sudo x230t_skulls.sh
```

**Note:** One step I had to do (as I was upgrading from an older *generic* x230 version to a *specific* x230t version) was to skip a compatibility check by altering the `-p internal` parameter to `-p internal:boardmismatch=force`.

Once completed, you'll now be prompted to shut the computer down. Reboot it and you'll be greeted by the new skulls version. Congratulations!

### GRUB
During the upgrade, I decided to use the `free` BIOS ROM image, which has the fortunate quirk of booting at native resolution immediately, which makes GRUB's 640x480 resolution look a bit unusual during switches.
This can easily be amended by editing `/etc/default/grub` and adding the line `GRUB_GFXMODE=1366x768x32`, before running `sudo grub-mkconfig -o /boot/grub/grub.cfg` to apply these changes.

### Downgrading
Unfortunately, not was all well with my upgrade. Even thought the process completed without issue I noticed strange issues with the computer and firefox crashing intermittently. I tried flashing the non-free image which gave me the exact same response.

So with only a few moments hesitation I decided to revert to version 0.1.8 again. The process is mostly the exact same as installation (albeit with a different release), however I did get a strange error while installing relating to `Reading old flash chip contents`. As [mentioned (and fixed) in this issue report](https://github.com/merge/skulls/issues/142#issuecomment-593694005), it can be fixed by simply replacing flashroms' `-p internal` parameter with with `-p internal:boardmismatch=force --force --noverify-all`.