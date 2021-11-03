---
title: "Disabling cgit scraping logs"
date: 2021-11-03T10:47:00
tags: ["Docker", "Linux", "Servers", "Snippets", "Software"]
---

One recurring problem that keeps happening every couple of months is my server will run out of disk space, the cause is usually the docker directory blowing up in size to a few gigabytes which on my small VPS can really start to cause issues.

You can find the offending containers using the excellent `ncdu` via:
```
sudo ncdu /var/lib/docker
```

When you have the ID of the container (e.g. `/var/lib/docker/overlay2/6fe41495127cc92398107df951416ec27463fd4ff6525a7d227bcf0c4e63803a`) you can find the corresponding container via:
```
for i in $(docker ps -a | awk '{if (NR!=1) {print $NF}}')
do
	if docker inspect "$i" | grep -q 6fe41495127cc92398107df951416ec27463fd4ff6525a7d227bcf0c4e63803a 
	then
		echo "$i"
	fi
done
```

With the offender found, you can start a shell in this container and browse to the files (in my case, /var/log)
```
docker exec -it cgit sh
cd /var/log/httpd/
ls -lah
```

Here I have a gigabyte `error_log` and a hundred megabyte `access_log`. Using `tail -f` to have a look at the files, it's mainly bots scraping diffs causing these logs.

Now let's get these disabled, there's a `robots=index, nofollow` option in `/etc/cgitrc` that can be changed to `robots=none`. To stop this option being reset on the container restarting, we'll mount this file to the host filesystem. Below are the relevant lines from my `docker-compose.yml` file:
```
    volumes:
      - $CONFDIR/cgit/cgitrc:/etc/cgitrc
```

As an added bonus, we can fix a long-standing issue with this container where code that should be highlighted is just blank. The line in question is `source-filter=/opt/highlight.sh` Comment out or remove this line and you'll have code previews working as expected.

Unfortunately, even with the above in place logs are immediately starting to fill up again with bot user agents. Time for a more janky solution! Logging is being controlled in this container via the `/etc/httpd/conf/httpd.conf` file, again we're going to mount this on the host filesystem with a `docker-compose` declaration:

```
    volumes:
      - $CONFDIR/cgit/cgitrc:/etc/cgitrc
      - $CONFDIR/cgit/httpd.conf:/etc/httpd/conf/httpd.conf
```

With this file on our host filesystem, we can now edit it. The offending lines are as follows:
```
ErrorLog "logs/error_log"
LogLevel warn

LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %b" common
CustomLog "logs/access_log" combined
```

All you need to do is pre-append all lines except ErrorLog with a `#` symbol, then change the `ErrorLog` location to `/dev/null`.

With that drastic and janky change, restart the container and you should notice that no more logs are being created.