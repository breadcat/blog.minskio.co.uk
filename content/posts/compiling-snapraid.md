---
title: "Compiling SnapRAID on Debian"
date: 2019-01-10T09:43:00
tags: ["guides", "linux", "servers", "snippets", "software"]
---

Recently I discovered [SnapRAID](http://www.snapraid.it/) as a parity based backup tool and found it to be extremely flexible and powerful, and is currently the backup solution I'm using on my own home server. While not suited to every use case, my current setup (rarely changing, incremental additions) fills the whole perfectly.

Debian doesn't include snapraid in it's repositories so we'll download and compile it from source, naturally!

Usually, I'd run this type of software through docker, however I generally prefer all underlying *core* services (samba/mergerfs/etc) to run on the host instead of inside a container. This may change in the future however!

First, we'll need to install some a couple of common dependencies

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gcc make wget
```

Then grab the [latest release](https://github.com/amadvance/snapraid/releases), at the time of writing is 11.3
```
wget https://github.com/amadvance/snapraid/releases/download/v11.3/snapraid-11.3.tar.gz
tar -xzf snapraid-11.3.tar.gz
cd snapraid-11.3
./configure
make
make check
sudo make install
```

Now, running `snapraid -V` should display the correct version number.

Once compiled and installed, you can go about [configuring and using](http://www.snapraid.it/manual) the program.
