---
title: "Manually fixing malformed AAC bitstreams"
date: 2019-10-22T09:21:00
tags: ["Formats", "Guides", "Media", "Snippets", "Software", "Windows"]
---

Recently while running `youtube-dl` on Windows, `ffmpeg` wasn't found in `%PATH%`, so it was unable to automatically fix the AAC bitstream. The file will play fine, but in the interest of completeness I still wanted this to be applied to my new files.

Not wanting to alter my path variable for a one-off fix, I investigated the source code a little and found the `FFmpegFixupM3u8PP` [function here](https://github.com/ytdl-org/youtube-dl/blob/3089bc748c0fe72a0361bce3f5e2fbab25175236/youtube_dl/postprocessor/ffmpeg.py#L577).

With that, you can now run:
```
ffmpeg -i "input_file.mp4" -c copy -f mp4 -bsf:a aac_adtstoasc "output_file.mp4"
```

And you're done.