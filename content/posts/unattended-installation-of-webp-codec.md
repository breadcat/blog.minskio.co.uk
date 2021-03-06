---
title: "Unattended installation of WebP codec"
date: 2020-05-26T16:48:00
lastmod: 2020-07-05T19:19:00
tags: ["Snippets", "Software", "Windows"]
---

I wrote this up many, many moons ago. I wanted a way to install Google's WebP codec on Windows in an unattended fashion to complement my [just-install](https://just-install.github.io/) script.

What I eventually came up with was the following:

```
rem cd to temp
cd %temp%

rem clean up old folders
del %temp%\WebpCodecSetup.exe %temp%\7z1701.msi %temp%\.data %temp%\.rdata %temp%\.reloc %temp%\.text
rd /s /q %temp%\7z1701 %temp%\.rsrc

rem download installer
bitsadmin /transfer installerDownload /download /priority normal https://storage.googleapis.com/downloads.webmproject.org/releases/webp/WebpCodecSetup.exe %temp%\WebpCodecSetup.exe

rem download 7zip
bitsadmin /transfer unpackDownload /download /priority normal http://www.7-zip.org/a/7z1701.msi %temp%\7z1701.msi

rem unpack 7zip
msiexec /a %temp%\7z1701.msi /qb TARGETDIR=%temp%\7z1701\

rem unpack installer
%temp%\7z1701\Files\7-Zip\7z.exe x %temp%\WebpCodecSetup.exe

rem rename and install source msis
ren %temp%\.rsrc\0\MSIFILE\10 10.msi
msiexec /i %temp%\.rsrc\0\MSIFILE\10.msi /quiet /qn /norestart

rem clean up used folders
del %temp%\WebpCodecSetup.exe %temp%\7z1701.msi %temp%\.data %temp%\.rdata %temp%\.reloc %temp%\.text
rd /s /q %temp%\7z1701 %temp%\.rsrc
```

The script will download the installer, and 7zip to do the unpacking, then delete everything afterwards.

Note, this will only install the 64bit install `10.msi`, if you're still working on 32bit Windows, you want to rename and install `1.msi` instead.

* **Edit 2020-07-05:** Changed instructions to only install 64bit version.
