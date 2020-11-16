---
title: "Compiling BSD tar on Linux"
date: 2018-11-09T12:09:00
tags: ["Guides", "Linux", "Snippets", "Software"]
---

While researching Arch Linux ARM for the ODroid HTPC project, I came across an issue with unpacking the image.

It turns out that Arch Linux ARM requires `bsdtar` version 3.3 or above, and only versions 3.2.x are available in the [Debian and Ubuntu repositories](https://packages.debian.org/search?keywords=bsdtar).

To resolve this, simply run the following:
```
sudo apt-get install build-essential
wget https://www.libarchive.org/downloads/libarchive-3.3.3.tar.gz
tar -xzf libarchive-3.3.3.tar.gz
cd libarchive-3.3.3/
./configure
make
sudo make install
bsdtar --version
```

Now you'll have the latest (at time of writing) version of <code>bsdtar</code> all working, all ready to proceed with the rest of the already convoluted installation guide.
