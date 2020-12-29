---
title: "Using NordVPN on ArchLinux"
date: 2020-12-03T13:43:00
tags: ["Linux", "Networks", "Snippets", "Software"]
---

I found myself with access to a NordVPN account a while back, and while VPN's have many, **many** downsides and the advertising of them is even worse, there is still a place for them.

The CLI client is available in the AUR, so it can be installed via:
```
yay -S nordvpn-bin
```

Once installed, you can manually start the daemon as root:
```
sudo nordvpnd
```

With the daemon started, you can login to your account, list available coutries to connect to and connect via:
```
nordvpn account
nordvpn countries
nordvpn connect Iceland
```

Once you've finished your nefarious activities, you can disconnect logically using:
```
nordvpn disconnect
```