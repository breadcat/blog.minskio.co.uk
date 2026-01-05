---
title: "Migrating from cgit to stagit"
date: 2025-07-14T12:57:00
lastmod: 2025-12-29T09:35:00
tags: ["Docker", "Infrastructure", "Linux", "Software"]
---

I've used [cgit](https://git.zx2c4.com/cgit/) for quite some time now and while it works for the most part it doesn't seem to work ...all that well. Every now and again I'll get errors like "An error occurred while reading CGI reply (no response received)" when opening repositories where the contents haven't changed for months or the container I use won't be able to be restarted without being recreated first.

This is probably not helped by me syncing all of these directories via syncthing, running `cgit` in a docker container or any number of other things but I'd like to try something simpler and a little lighter if possible.

In steps `stagit`, a [static git page generator](https://codemadness.org/stagit.html) which seems more up my street.

Initially I had issues running this as it isn't availabke in the ArchLinux ARM package repositor, and the [AUR pkgbuild](https://aur.archlinux.org/packages/stagit-git) doesn't compile on `aarch64` or even manually so that again is probably an issue with my setup. In a very prophetical way, I ended up using Nix to run the software which just plowed on without any issues after [being installed](https://nixos.org/download/#nix-install-linux) via:

```
nix-env -f '<nixpkgs>' -iA stagit
```

Then I built a [quick script](https://github.com/breadcat/Dockerfiles/commit/6699a22d7a18b8b69d3860d93be364b213776478) to loop through my `source_directory`, running the program on any git repositories it finds before generating an index for easier browsing.

After this, all that's needed is to serve the directory via a simple nginx container and you're pretty much done.

* **Edit 2025-12-29:** The above script has now been replaced [by a NixOS equivalent](https://github.com/breadcat/nix-configs/blob/main/scripts/stagit-generate.nix) with inline CSS using heredocs.