---
title: "Windows 10 Network Share Guest Access"
date: 2019-02-15T11:47:00
tags: ["servers", "snippets", "software", "windows"]
---

I recently re-installed Windows 10, my edition of choice was the new 2019 edition of LTSC-N and as such I updated the [prep script](https://github.com/breadcat/win10-prep) I use to *provision* a new installation.

One thing worth noting however was that I couldn't access this script stored on a remote Linux Samba share. After checking the server everything looked correct until I looked at the group policies enabled by default in this new LTSB/LTSC version.

The following registry key needs altering to allow *insecure* guest access to these shares.

```
reg add "HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" /v "AllowInsecureGuestAuth" /t REG_DWORD /d "1" /f
```

With the change made, everything will work as expected. Nicely done.