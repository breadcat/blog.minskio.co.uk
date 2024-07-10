---
title: "The briefest Arch Linux installation guide"
date: 2019-10-15T14:33:00
lastmod: 2024-07-10T12:07:00
tags: ["Guides", "Linux", "Snippets", "Software"]
---

Are you wanting an incredibly quick (to the point of being useless, or painfully truncated) guide for how to install Arch Linux on your computer?

Do you want to throw caution to the wind, booting from UEFI using systemd-boot instead of the more usual GRUB? Are you interested in Wayland/Sway instead of a more traditional Xorg/i3? Do you want to use a swap file instead of a partition? Do you desire UK specific keyboard layouts and timezone, even if you don't want them?

Well why didn't you say? Step aboard traveller..! Welcome to my personal guide (primarily for reference usage) on installing the venerable Arch Linux.

* Boot your installation medium

* Check for network connectivity `ping archlinux.org`.

* Update your package manager repository `pacman -Syy`.

* Set the keymap to UK `loadkeys uk`.

* Make sure the time and date are vaguely correct `timedatectl set-ntp true`.

* Identify the drives you're working with (in my case, sda) using `lsblk`.

* Now make an EFI boot partition, and root partition `gdisk /dev/sda > o y n 1 {enter} 512M EF00 n 2 {enter} {enter} {enter} w y`.

* Check everything's okay with the above via `gdisk -l /dev/sda`.

* Format those new partitions `mkfs.vfat /dev/sda1 && mkfs.ext4 /dev/sda2`.

* Now mount those partitions `mount /dev/sda2 /mnt && mkdir /mnt/boot && mount /dev/sda1 /mnt/boot`.

* Install the base operating system `pacstrap /mnt/ base base-devel dhcpcd linux linux-firmware neovim openssh`.

* Create your `fstab` file using `genfstab -U /mnt > /mnt/etc/fstab`.

* Chroot into it `arch-chroot /mnt/`.

* Enable DHCP `systemctl enable dhcpcd`.

* Now install and configure your (UEFI) bootloader `bootctl install`.

* Configure said bootloader default `printf "default arch\ntimeout 2">/boot/loader/loader.conf`.

* Now add your Arch entry `printf "title ArchLinux\nlinux /vmlinuz-linux\ninitrd /initramfs-linux.img\noptions root=/dev/sda2 rw">/boot/loader/entries/arch.conf`.

* Set your hostname `printf "minerva">/etc/hostname`.

* Set your console keymap `printf "KEYMAP=uk">/etc/vconsole.conf`.

* Set the locale language `printf "LANG=en_GB.UTF-8">/etc/locale.conf`.

* Another locale step, I guess `nvim /etc/locale.gen` you want to uncomment `en_GB.UTF-8`.

* Set up a symlink for the timezone `ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime`.

* Finally we can generate the locale `locale-gen`.

* Add your non-root user `useradd -m -G wheel,users -s /bin/bash peter`.

* Set your new accounts' password `passwd peter`.

* Set up sudo `EDITOR=nvim visudo`, you'll need to uncomment the `%wheel` group line.

* Set your root password `passwd`.

* Exit the chroot environment `exit`.

* Lastly, reboot from to your new OS `reboot`.

Now we're logged in, we can [set up the swap file](/creating-a-swap-file-on-linux/) as an alternative to a swap partition (mentioned earlier).

Lastly, I'd like to autologin my user, without using a display manager:
```
sudo mkdir -p /etc/systemd/system/getty@tty1.service.d
printf "[Service]\nExecStart=\nExecStart=-/usr/bin/agetty --autologin username --noclear %%I $TERM" | sudo tee /etc/systemd/system/getty@tty1.service.d/override.conf
```
Replacing *username* with whatever the use you want to login as.

* **Edit 2019-10-21:** Add update package manager repo step, may resolve some problems

* **Edit 2019-10-24:** Add [base package](https://www.archlinux.org/news/base-group-replaced-by-mandatory-base-package-manual-intervention-required/), swap file link and autologin instructions

* **Edit 2020-01-20:** Using ext4 over btrfs, the less said about it the better.

* **Edit 2020-01-21:** Alternate instructions for using BIOS/Grub are below:
```
fdisk /dev/sda > a {enter} w {enter}
sudo pacman -S grub
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

* **Edit 2020-06-03:** Added fstab instruction, seemingly missed.

* **Edit 2020-06-17:** Replaced nano references with neovim.

* **Edit 2020-06-28:** Merge pacstrap and pacman commands.

* **Edit 2024-07-10:** Add openssh to initial pacstrap.
