---
title: "PS2 Image CSO Compression on Linux with MaxCSO"
date: 2018-11-27T10:23:00
tags: ["Formats", "Guides", "Linux", "Snippets", "Software"]
---

CSO was originally only intended for smaller PS1/PSP disc images, so a different version can be compiled to support the larger DVD based images that PS2 games are usually distributed on.

```
sudo apt-get install git-core build-essential liblz4-dev libuv1-dev zlib1g-dev
git clone https://github.com/unknownbrackets/maxcso.git
cd maxcso
make
sudo make install
```

Now that maxcso has been compiled and installed, simply compress all iso images in a directory via:

```
for i in *.iso; do maxcso "$i"; done
```
