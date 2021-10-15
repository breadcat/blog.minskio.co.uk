---
title: "Replacing youtube-dl with yt-dlp"
date: 2021-09-18T17:58:00
tags: ["Media", "Linux", "Snippets", "Software"]
---

At the time of writing, [youtube-dl](https://github.com/ytdl-org/youtube-dl) hasn't been updated since July 2021 and has many unresolved issues, some of which cause real problems with my RSS/YouTube workflow. In steps [yt-dlp](https://github.com/yt-dlp/yt-dlp) to the rescue which fixes the issues I've experienced and continues to be updated to this day.

Now while I usually like to keep as many items as possible managed via my package manager (and there are yt-dlp [AUR packages available](https://aur.archlinux.org/packages/?K=yt-dlp)), youtube-dl was one of the exceptions I'd make. Usually I'll stick to the same version until I need to manually update for a broken website.

Uninstall any older versions you have floating around on your system and install the latest binary from GitHub and make it executable:
```
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp
```

Now you can symlink the new binary to the old name to emulate any calls to `youtube-dl` with `yt-dlp` using:
```
sudo ln -s /usr/local/bin/yt-dlp /usr/local/bin/youtube-dl
```

Now ensure you can update the binary using
```
sudo youtube-dl -U
```

And you're done.