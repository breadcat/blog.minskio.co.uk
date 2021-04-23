---
title: "FFmpeg audio conversions for PBX"
date: 2019-01-14T16:08:00
tags: ["Formats", "Guides", "Linux", "PBX", "Snippets", "Windows", "Work"]
---

Firstly, grab a copy of `ffmpeg` from the [official website](https://www.ffmpeg.org/download.html#build-windows) if you're running Windows, or your package manager if you're running Linux.

## Samsung OfficeServ
```
ffmpeg -i "input.ext" -codec pcm_s16le -ar 8000 -ac 1 -ab 128k "output.wav"
```

### LG/Ericsson eMG80
```
ffmpeg -i "input.ext" -codec:a pcm_mulaw -ar 8000 -ac 1 -ab 64k "output.wav"
```

### LG/Ericsson Hosted Platform
```
ffmpeg -i "input.ext" -codec:a pcm_s16le -ar 8000 -ac 1 -ab 64k "output.wav"
```

### Gamma Horizon Hosted Platform
```
ffmpeg -i "input.ext" -codec:a pcm_mulaw -ar 8000 -ac 1 -ab 64k "output.wav"
```

Sometimes, the files can be a little loud when played. You can simply knock this down during conversion via `-af volume=0.8` which predictably equates to 80% of the original volume