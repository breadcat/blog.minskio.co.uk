---
title: "Flashing Skulls to a Thinkpad x230t"
date: 2020-01-21T11:22:00
tags: ["Guides", "Hardware", "Linux", "Software"]
---

I've always been quite interested in the [Coreboot project](https://www.coreboot.org/), and my laptop of choice (the [Lenovo Thinkpad x230t](/hardware/)) semeed to *almost* be supported. When I learned about the [Skulls project](https://github.com/merge/skulls) it seemed like a perfect match.

After a bit of research and on the advice of a few people who had gone through the process previously I picked up a CH341A USB programmer instead of the usual Raspberry Pi to do the actual flashing. It turns out however you don't need any specialised hardware at all and the entire process can now be done in software using an exploit and the excellent [1vyrain](https://github.com/n4ru/1vyrain).

# Preparing your laptop
Like an idiot, many months ago I'd upgraded my BIOS version to the absolutely latest version offered by Lenovo which at the time of writing is 2.75. The exploit 1vyrain uses is only compatible with versions 2.58 and below.
The official word from Lenovo is that you're unable to downgrade your BIOS after specific versions however this is false as stated in [this helpful guide](https://github.com/gch1p/thinkpad-bios-software-flashing-guide#downgrading-bios).

The long winded way I got my BIOS downgraded was to write a bootable FreeDOS installation to my USB stick, using [Rufus](https://rufus.ie/) on Windows. The only files required on top of this is a compatible version of `dosflash.exe` and your BIOS image.

You can grab a compatible `dosflash.exe` by extracting the binary from the [BIOS upgrade iso](https://pcsupport.lenovo.com/gb/en/products/laptops-and-netbooks/thinkpad-x-series-tablet-laptops/thinkpad-x230-tablet/downloads/ds029683) file using 
```
geteltorito -o out.img g2uj15us.iso
mount -t vfat bios.img bios/ -o loop
```
Inside this mounted image you can grab dosflash and copy it to the root of your FreeDOS USB stick. Lastly you want a compatible BIOS image. Initially there was no support for the x230t model, but this has been added with [this pull request](https://github.com/n4ru/IVprep/pull/3) so you can grab the file directly [from here](https://github.com/n4ru/IVprep/raw/master/BIOS/X230t.FL1).

Now boot up FreeDOS and flash the old BIOS image using:
```
dosflash.exe /sd /file X230t.FL1
```

Confirm you're running a compatible BIOS version and move on to the next step.

# Preparing Skulls
I'm not going to compile Skulls myself, it's an additional step that I would rather not deal with. I downloaded [the latest version](https://github.com/merge/skulls/releases/latest) and extracted the **non-free** `top.rom` file to an accessible directory on my webserver. More on this later.

# Flashing Skulls
1vyrain offers a [live USB image](https://n4ru.it/1vyrain/) for doing all required leg work and applying the exploit.
Now boot your img file in UEFI mode and follow the onscreen instructions. Eventually you'll be able to specify your own image to flash which is where the file we prepared above comes in. Enter the path for this and flash the image and reboot when prompted.

# First Boot
Your laptop was likely previously booting in UEFI mode, but the included SeaBIOS image will only boot in BIOS mode so you're going to need to chroot into your install and install a compatible bootloader. I was in the mood for a clean install anyway so just installed `grub` on my Linux install.

I did plan to look into preparing a [TianoCore](https://www.tianocore.org/) or [YaBits](https://yabits.github.io/) payload to support UEFI, but BIOS works well enough and I don't care enough to compile my own Skulls images.


And with that, an entry from the [New Years Resolution list](/blog/2020/01/02/2020-new-years-resolutions/) is complete.
<!-- # Cleaning ME -->

