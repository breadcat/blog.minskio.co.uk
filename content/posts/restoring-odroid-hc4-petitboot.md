---
title: "Restoring PetitBoot on an ODROID-HC4"
date: 2022-07-20T09:53:00
tags: ["Linux", "Snippets", "Software"]
---

In the process of messing around with a few different Operating Systems (before eventually settling on Debian Netboot) I'd managed to render the device unbootable. All I'd get upon powering up the device was a solid red LED with no display, after Googling around a little and browsing the forums it looked as though I needed to restore the original bootloader.

At the time of writing, the two latest SPI files are `spiupdate_odroidhc4_20201222.img.xz` and `spiboot-20220317.img`. Download both files [from this server](http://ppa.linuxfactory.or.kr/images/petitboot/odroidhc4/), then extract and write your `spiupdate_odroidhc4_20201222.img` to a Micro SD card either using `dd` or [Rufus](http://rufus.ie/en/). Once written, rename you `spiboot-20220317.img ` to `spiboot.img` and replace the file in the root of the SD card with this replacement.

Now boot from this SD card, you may need to hold down the black button on the underside when powering on. Once you're in the temporary PetitBoot environment, select 'Exit to Shell' and then run `pb-update`. This will go online and update/restore your PetitBoot image. Once done you should now be able to reboot without the SD card straight into PetitBoot, solving the initial problem.
