---
title: "Upgrading Ubiquiti Routers Over SSH"
date: 2019-03-27T14:55:00
tags: ["Guides", "Hardware", "Linux", "Networks", "Servers", "Software"]
---

In the past, I usually check for ubiquiti router firmware updates via [RSS feed](https://community.ubnt.com/ubnt/rss/board?board.id=Blog_EdgeMAX), and to apply them I open a reverse SSH tunnel to the web interface and upload the file over this tunnel.
This is somewhat inefficient however as you're uploading firmware over a few additional links in a chain that aren't really required.


This guide is a simple repurposing of the [following help article](https://help.ubnt.com/hc/en-us/articles/205146110-EdgeRouter-How-to-Upgrade-the-EdgeOS-Firmware#3), albeit somewhat abridged.

To solve this (and speed up the process in general) you can do the full upgrde via SSH.

Firstly, you'll want to SSH into your router:
```
ssh username@routerip
```
The default username and password are both `ubnt`, but it's highly recommended you change this.

Then check the currently running firmware version via:
```
show version
```

Then upload the image
```
add system image https://dl.ubnt.com/firmwares/edgemax/v2.0.x/ER-e100.v2.0.1.5174691.tar
```
Verify the firmware is the correct version for the model of router you're using.

Then check the image is installed and will be booted from:
```
show system image
```

Lastly, reboot the router to apply the image:
```
reboot
```

And you're done.