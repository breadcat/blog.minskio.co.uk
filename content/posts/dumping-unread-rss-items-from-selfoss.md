---
title: "Dumping unread RSS items from Selfoss"
date: 2021-03-05T09:55:00
tags: ["Databases", "Guides", "Linux", "Servers", "Snippets", "Software"]
---

I've recently gone through somewhat of a RSS (r)evolution recently, finding myself switching from the excellent web based [Selfoss](https://selfoss.aditu.de/) to the command line [Newsboat](https://newsboat.org/) application and then wanting multiple clients across multiple platforms (without awkwardly syncing a database) using [Tiny Tiny RSS](https://tt-rss.org/) as my server, with Newsboat, the Web UI and the [FOSS Android client](https://f-droid.org/en/packages/org.fox.tttrss/) all as clients working flawlessly.

During migrations around the place however you'll inevitably end up with multiple running RSS readers, all updating independently, duplicating unread items. An easy solution to this that I overlooked for far far too long, is to just dump your unread items to a text file, and work through them at your own pace.

To do this, follow away. I was using the [docker container](https://hub.docker.com/r/hardware/selfoss) so my persistent storage database `selfoss.db` was mounted on my host file system which made finding and copying it incredibly easy. Move this to a location where you can work on it, then install the `sqlite` binary using your package manager.

You can now dump your unread items using:
```
sqlite3 -csv -noheader selfoss.db "select author, title, link from items where unread = 1;" > selfoss.csv
```

You can tidy up the formatting a little with sed, but I just sort and deduplicate the list with `uniq` and start whittling away.