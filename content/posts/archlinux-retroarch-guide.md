---
title: "ArchLinux Retroarch Guide"
date: 2019-02-19T09:46:00
tags: ["Emulation", "Games", "Linux", "Media", "Snippets", "Software"]
---

### Installation
The latest retroarch can be installed on ArchLinux simply with the following command:
```
pacman -S retroarch
```
No need to install any other packages, the online updater (below) can take care of everything.

Launch `retroarch` to generate all your directories and config files. Exit by pressing the Esc key, and make the alterations below.

### Configuration Changes
Edit `$HOME/.config/retroarch/retroarch.cfg` with your text editor of choice. Below are the values I've altered.
```
assets_directory = "~/.config/retroarch/assets"
libretro_directory = "~/.config/retroarch/cores"
libretro_info_path = "~/.config/retroarch/cores"
menu_show_core_updater = "true"
rgui_browser_directory = "~/media/games"
savefile_directory = "~/media/games/.saves"
savestate_directory = "~/media/games/.saves"
system_directory = "~/media/games/.firmware"
systemfiles_in_content_dir = "false"
video_fullscreen = "true"
```
Save the file and move on to the next step.

### Assets
Now assets can be saved to a writeable directory, so start retroarch again and update the following:
* Retroarch > Online Updater > Update Assets
* Retroarch > Online Updater > Update Joypad Profiles
* Retroarch > Online Updater > Update Core Info Files

### Cores
Lastly, you want to select the cores you want to use, I have the following (usually *best-in-slot*) cores installed:
* MAME
* Stella
* Hatari
* Beetle PCE
* Citra
* melonDS
* SameBoy
* mGBA
* Dolphin
* Mesen
* ParaLLEl N64
* Snes9x
* Flycast
* Genesis Plus GX
* Beetle Saturn
* Beetle PSX HW
* Play!
* PPSSPP

### Updates

* **Edit 2020-01-27:** Changed a couple of core recommendations, removed section on updating cores using [lrcm](https://github.com/meleu/lrcm)
* **Edit 2020-02-04:** Altered some paths
* **Edit 2020-05-19:** Updated core selections

