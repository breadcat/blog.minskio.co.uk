---
title: "Debloating the LG Q6 Phone"
date: 2020-08-25T13:17:00
tags: ["Android", "Guides", "Linux", "Minimalism", "Software", "Snippets"]
---

I've had the LG Q6 for many moons now, I picked it up used for about 120GBP and it fits right in with my cheap and cheerful mantra. One thing that's always annoyed me about it however was the inability to root the device and install a custom ROM with less bloat! The bootloader is locked, and any guides you'll find online are copy/paste jobs for other models that don't work. You can hide apps and even disable them, but that doesn't uninstall them.

Anyway, Chris Titus recently published [a video](https://www.youtube.com/watch?v=k9ErL9L6KIw) and [matching article](https://christitus.com/debloat-android/) that ticked all my boxes. My guide is basically a rehash of his, so I recommend you go read his before seeing the list of things I removed.

Without further ado, you'll want to install ADB tools:
```
sudo pacman -S adb
```

Then enable developer mode on your phone (Tap Build Number 7 times in About > Software). Connect up a USB cable to your phone and start the make sure your phone isn't `unauthorized`. I had to set my phone to Charding Mode only for it to appear authorized.
```
adb devices
```

With this, you can now list out your installed packages:
```
adb shell pm list packages
```

As we don't have root, you can't globally uninstall applications using `adb uninstall packagename`, we instead need to delete it for our user with `adb shell pm uninstall --user 0 packagename`.

I don't use many Google products, so I uninstalled swathes of Android without any serious side effects. The full list of what I removed is...

```
adb shell pm uninstall --user 0 com.android.calendar
adb shell pm uninstall --user 0 com.android.cellbroadcastreceiver
adb shell pm uninstall --user 0 com.android.chrome
adb shell pm uninstall --user 0 com.android.gallery3d
adb shell pm uninstall --user 0 com.android.LGSetupWizard
adb shell pm uninstall --user 0 com.android.wallpapercropper
adb shell pm uninstall --user 0 com.facebook.appmanager
adb shell pm uninstall --user 0 com.facebook.system
adb shell pm uninstall --user 0 com.google.android.apps.docs
adb shell pm uninstall --user 0 com.google.android.apps.docs.editors.docs
adb shell pm uninstall --user 0 com.google.android.apps.docs.editors.sheets
adb shell pm uninstall --user 0 com.google.android.apps.docs.editors.slides
adb shell pm uninstall --user 0 com.google.android.apps.photos
adb shell pm uninstall --user 0 com.google.android.apps.tachyon
adb shell pm uninstall --user 0 com.google.android.gm
adb shell pm uninstall --user 0 com.google.android.googlequicksearchbox
adb shell pm uninstall --user 0 com.google.android.music
adb shell pm uninstall --user 0 com.google.android.videos
adb shell pm uninstall --user 0 com.google.android.youtube
adb shell pm uninstall --user 0 com.lge.bnr
adb shell pm uninstall --user 0 com.lge.bnr.launcher
adb shell pm uninstall --user 0 com.lge.email
adb shell pm uninstall --user 0 com.lge.equalizer
adb shell pm uninstall --user 0 com.lge.eula
adb shell pm uninstall --user 0 com.lge.eulaprovider
adb shell pm uninstall --user 0 com.lge.exchange
adb shell pm uninstall --user 0 com.lge.faceglance.enrollment
adb shell pm uninstall --user 0 com.lge.faceglance.trustagent
adb shell pm uninstall --user 0 com.lge.fmradio
adb shell pm uninstall --user 0 com.lge.gallery.collagewallpaper
adb shell pm uninstall --user 0 com.lge.gametuner
adb shell pm uninstall --user 0 com.lge.hifirecorder
adb shell pm uninstall --user 0 com.lge.ia.task.incalagent
adb shell pm uninstall --user 0 com.lge.ia.task.informant
adb shell pm uninstall --user 0 com.lge.ia.task.smartcare
adb shell pm uninstall --user 0 com.lge.ime
adb shell pm uninstall --user 0 com.lge.ime.solution.handwriting
adb shell pm uninstall --user 0 com.lge.lgworld
adb shell pm uninstall --user 0 com.lge.music
adb shell pm uninstall --user 0 com.lge.phonemanagement
adb shell pm uninstall --user 0 com.lge.qmemoplus
adb shell pm uninstall --user 0 com.lge.signboard
adb shell pm uninstall --user 0 com.lge.sizechangable.musicwidget.widget
adb shell pm uninstall --user 0 com.lge.sizechangable.weather
adb shell pm uninstall --user 0 com.lge.sizechangable.weather.platform
adb shell pm uninstall --user 0 com.lge.sizechangable.weather.theme.optimus
adb shell pm uninstall --user 0 com.lge.smartdoctor.webview
adb shell pm uninstall --user 0 com.lge.theme.black
adb shell pm uninstall --user 0 com.lge.theme.highcontrast
adb shell pm uninstall --user 0 com.lge.theme.titan
adb shell pm uninstall --user 0 com.lge.theme.white
adb shell pm uninstall --user 0 com.lge.videoplayer
adb shell pm uninstall --user 0 com.lge.videostudio
```

You'll **very, very** likely want to look up what most of these applications are/do before just blindly running the above commands.

Lastly, in the event you ever uninstall something you wanted to keep, you can re-install the application by issuing:
```
adb shell cmd package install-existing application_name
```

* **Edit 2020-10-26:** Add reinstall instructions, remove calendar and email applications.