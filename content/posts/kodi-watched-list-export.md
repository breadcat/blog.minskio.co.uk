---
title: "Exporting Kodi Watched Status to Markdown"
date: 2019-01-31T10:25:00
tags: [ "Guides", "Kodi", "Linux", "Media", "Movies", "Servers", "Snippets", "Software" ]
---

As I've outlined in [this page](/archived-movies/) I'd prefer to save space on my server and delete movies once I've seen them but also keep a log so I don't need to remember everything.

Step in the Kodi plugin [WatchedList](https://kodi.wiki/view/Add-on:WatchedList) which will happily export your status to a SQLite database that can be worked with as follows:
```
if [ -f "movies.csv" ]; then rm movies.csv; fi
sqlite3 -noheader -csv watchedlist.db "select title from movie_watched;" > movies.csv
sed -i -e 's|\"||g' -e 's|^|* |g' movies.csv
sort -k 2 < movies.csv > movies.md
rm movies.csv
```

One thing you may notice, is movies being released in year '65535'. This is caused by your Kodi library itself and can be sorted by refreshing the title to grab new (and hopefully correct) metadata.

You can also run the below to get a list of TV shows that have watched episodes. I'm unable to find an efficient way of getting completely watched TV shows, but the below will get you halfway there:
```
sqlite3 -noheader -csv watchedlist.db "select * from tvshows;" > tv_shows_index.csv
watched_id=$(sqlite3 -noheader watchedlist.db "select idShow from episode_watched;" | uniq)
for i in $watched_id; do grep $i tv_shows_index.csv | cut -f2 -d, >> tv_shows.csv; done
sed -i -e 's|\"||g' -e 's|^|* |g' tv_shows.csv
sort -k 2 < tv_shows.csv > tv_shows.md
rm tv_shows.csv tv_shows_index.csv
```