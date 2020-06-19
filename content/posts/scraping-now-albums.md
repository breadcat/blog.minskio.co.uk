---
title: "Scraping and Grabbing Now! albums"
date: 2018-12-04T16:28:00
tags: ["Guides", "Linux", "Lists", "Music", "Servers", "Snippets", "Software"]
---

Recently a collegue at work came to me to download them an album from online, unfortunately as it was a compilation album and the individual tracks had been released a million times already this wasn't to be released through the usual channels.

No matter though, vague scripting to the rescue! The tracklist that I was after was available on the [now website](https://www.nowmusic.com/album/now-rock-n-roll/) which had no issues being scraped.

```
source=$(wget https://www.nowmusic.com/album/now-rock-n-roll/ -qO-)
artists=$(printf "$source" | grep artist | sed 's/^.*>\([^<]*\)<.*$/\1/')
titles=$(printf "$source" | grep \"title\" | sed 's/^.*>\([^<]*\)<.*$/\1/')
paste <(printf "$artists") <(printf "$titles") | sed -e 's/\t/ - /g' > parse_list.txt
```

Now we have all 73 tracks in a single text file, no fuss, no muss.

All of these tracks are incredibly likely to be uploaded to youtube, so we can grab them using the ever-excellent `youtube-dl`

To manage this, we'll run a youtube search on every entry, and grab the resulting output, converting it to `mp3` along the way.

```
while read line; do youtube-dl -x --audio-format=mp3 ytsearch:"$line lyrics"; done < parse_list.txt
```

Please note, I append a " lyrics" in the search string to avoid too obvious music videos that sometimes have 

With this, we have 73 `mp3` files dumped into our working directory with messy filenames. I usually throw these into `beets` in singleton mode via docker to improve the quality of the filenames/tags.

```
docker run -it -v $(pwd):/music linuxserver/beets bash
beet im -s /music
```

This will take some time, and will need a lot of nannying as there are no existing tags to work with initially. After the process however you'll be rewarded with tagged files ready to (rock 'n) roll.