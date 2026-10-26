---
title: "Disabling rotation suggestions on Android"
date: 2026-10-20T16:49:00
tags: ["Android", "Linux", "Snippets", "Software"]
---

I use GrapheneOS on my Pixel 7 phone with screen rotation disabled and have done since the moment I got this phone.

If I'm using it while lying down however, I get a ~~helpful~~ *infuriating* little floating button at the bottom of the phone which offers to rotate my screen. Accidentally pressing this will rotate the screen but locking it in place. There's a way to disable this however instead of being a toggle in the settings menu, you need to delve in using `adb`.

As a certified masochist, I'm naturally doing all of this over Wifi on my distro of choice, NixOS.

Firstly, you'll need to enable debugging on your phone:

* Enable developer options\
About phone > tap Build number 7 times
* Developer options > Debugging > Wireless Debugging > Enable
* Allow over your SSID

Then, on the computer front, we're connecting via ADB:

```
nix-shell -p android-tools
adb pair your.phone.ip.address:port
adb connect your.phone.ip.address:second-port-number
adb devices
adb shell settings put secure show_rotation_suggestions 0
adb disconnect
exit
```

And you're done, enjoy being horizontal.
