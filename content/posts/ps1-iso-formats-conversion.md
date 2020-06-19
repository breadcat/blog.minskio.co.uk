---
title: "Sony PS1 Image Conversion Guide"
date: 2020-05-03T16:52:00
tags: ["Formats", "Guides", "Linux", "Snippets", "Software"]
---


Every now and then, when acquiring  a PlayStation game from nefarious sources you'll be presented with a folder full of mysterious files instead of a single expected single iso image or friendly bin/cue pair or chd.

There are plenty of guides out there for Windows but none really covering the Linux side of things. This guide assumes you're running Debian, or at least have access to the Debian repository of software.

### .7z
```
apt-get install p7zip
for i in *.7z; do p7zip -d "$i"; done
```

### .rar
```
apt-get install unrar
for i in *.rar; do unrar x "$i"; done
```

### .zip
```
apt-get install unzip
for i in *.zip; do unzip "$i"; done
```

### .ape
```
apt-get install ffmpeg
for i in *.ape; do ffmpeg -i "$i" -f s16le "${i%ape}bin" && rm "$i"; done
```

### .ecm
```
apt-get install ecm
for i in *.ecm; do ecm-uncompress "$i" && rm "$i"; done
```

### Missing .cue
Sometimes (and annoyingly) these game packages won't include their associated cue sheet, the majority can be found in [this collection](https://github.com/opsxcq/psx-cue-sbi-collection/).
Alternatively, you can [create one](https://github.com/opsxcq/psx-cue-sbi-collection/#generating-a-generic-cue-file) if there is only a single track in the disc image.

### Conversion to .chd
One last step that I quite like to perform is to convert this bin/cue pair into a single .chd file.
Support for this file format is quite good albeit lacking from mednafen (as is the less-supported and documented pbp format) however is present in the Retroarch port, [beetle-psx](https://github.com/libretro/beetle-psx-libretro).
```
apt-get install mame-tools
for i in *.cue; do chdman createcd -i "$i" -o "${i%cue}chd"; done
```

### Playlist .m3u files
Playlist support is one thing lacking in the chd  format currently.
If you have a multi-disc games, such as Metal Gear Solid and you'd like to take advantage of disc swapping facilities provided by your emulator you can create a playlist file containing your image files.
For the Metal Gear Solid example, your m3u file will simply be called Metal Gear Solid.m3u  and it's contents will be:
```
Metal Gear Solid.cd1.chd
Metal Gear Solid.cd2.chd
```

### SquashFS alternative
Alternatively, if you'd still like to run your games via vanilla `mednafen` or just want to compress your game images into a single archive, you can do the following:
```
apt-get install squashfs-tools
mksquashfs sony_playstation_games_directory sony_playstation.squashfs -b 1048576 -comp xz -Xdict-size 100%
```

And then, to mount the archive:
```
mkdir sony_playstation
mount sony_playstation.squashfs sony_playstation -t squashfs -o loop
```