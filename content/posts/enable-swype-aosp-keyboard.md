---
title: "Enabling Swype on the AOSP keyboard"
date: 2021-01-31T14:42:00
lastmod: 2021-02-03T11:51:00
tags: ["Android", "Linux", "Snippets", "Software"]
---

I've recently changed my phone to a Pixel 4, one of the reasons for this was to have as much of an open source device as possible. Naturally my operating system of choice was [LineageOS 17](https://lineageos.org/).

With this however, the included AOSP keyboard will not support gesture inputs without a proprietary Google library. To get this working, you'll first want to download the [Pico package of Open GApps](https://opengapps.org/) for your phone, I'm using ARM64 architecture for my Pixel 4 device however yours may be different.

Once downloaded, extract your archive:
```
unzip open_gapps-arm64-9.0-pico-20210127.zip
```

Now move to the directory containing the library and extract that:
```
cd Optional
tar --lzip -xf swypelibs-lib-arm64.tar.lz
cd swypelibs-lib-arm64/common/lib64
ls
```

Here, you should now have `libjni-latinimegoogle.so`. With this, you want to push this library to your phone.
Once connected via ADB (and with root access enabled in the debugging menu), do the following:
```
adb devices
adb root
adb remount
adb push libjni_latinimegoogle.so /system/lib64/
adb unroot
adb reboot
```

Once rebooted, browse to:
```
Settings > System > Languages & input > Virtual Keyboard > Android Keyboard (AOSP) > Gesture Typing > Enable gesture typing
```

One annoyance is that whenever a OTA update is pushed out to your device, it will remove this library. A [helpful script](https://gist.github.com/rugk/a4c9fa11c5c031faf45602d6bf922a1c) ([and instructions](https://android.stackexchange.com/a/187686)) to workaround this have been provided by [rugk](https://github.com/rugk):
```
wget "https://gist.github.com/rugk/a4c9fa11c5c031faf45602d6bf922a1c/raw/eed64cea4d37f5c35cd4ced5ceb0281fef38afc9/95-latinimegoogle.sh"
sed -i 's/lib\//lib64\//g' 95-latinimegoogle.sh
adb root
adb remount
adb push 95-latinimegoogle.sh /system/addon.d/
adb shell
cd /system/addon.d
chmod +x 95-latinimegoogle.sh
exit
adb unroot
adb reboot
```

And you should be done.

* **Edit 2021-02-03:** Add OTA update library preservation section.
