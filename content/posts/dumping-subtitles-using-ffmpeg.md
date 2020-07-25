---
title: "Dumping subtitles using FFmpeg"
date: 2020-05-17T14:00:00
tags: ["Formats", "Languages", "Linux", "Media", "Snippets", "Software"]
---

As part of my ongoing [language learning](/languages/) attempts, I tend to enable subtitles in the language I'm wanting to learn, and when I see a word I don't recognise in the context I do I'll make a note of it.

This manual approach though can get a bit tiresome, so if you want a quick way to dump an entire subtitle file from a video, you can do the following:
```
ffmpeg -i video.mkv
```

This will list the streams available, video, audio and subtitles if available. If labels are available it will show them here too. Make a note of the stream you want. In the above example, we're seeing `Stream #0:5(nor)` in our original command output which would be Norwegian. To dump this stream to a subrip `srt` file you'll run the following:
```
ffmpeg -i video.mkv -c copy -map 0:5 subtitles.srt
```

You should now have a single `srt` file with your subtitles as expected in them.

I'll write another post for how to format these files to a usable list type format later.
**Edit 2020-07-25:** [Follow up article posted here](/formatting-dumped-subtitles-into-a-vocabulary-list/).