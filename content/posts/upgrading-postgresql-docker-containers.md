---
title: "Upgrading PostgreSQL in an Alpine docker container"
date: 2021-10-18T17:16:00
lastmod: 2023-02-14T22:37:00
tags: ["Databases", "Docker", "Guides", "Linux", "Servers", "Software"]
---

I use PostgreSQL as the database backend for my RSS reader, [Tiny Tiny RSS](https://tt-rss.org/). In a potentially misguided leap, I run this inside a [docker container](https://git.minskio.co.uk/cgit.cgi/dockerfiles/.git/plain/docker-compose.yml) that will automatically upgrade when a new release comes about.

This guide assumes you have an `postgres:alpine` container named `postgres`, and the user is also named `postgres`. I'm also managing this using `docker-compose`. If not, amend the guide where necessary. The upgrade I performed was version 13 to 14, but this guide should be version agnostic.

Firstly, let's back up the existing container volume just in case anything goes wrong:
```
XZ_OPT=-9 sudo tar cJf postgres-backup.tar.xz postgres/
```

Now with that crude backup, we need to dump our database properly to an SQL file.

In its' current state, the database is continually restarting as the server and database versions don't match. You can bring this back up by changing the tag in your compose file from `postgres:alpine` to `postgres:13-alpine`.
When the container is running, you can now export the database to your host filesystem using:
```
docker exec -it postgres pg_dumpall -U postgres > postgres-dump.sql
```

Now we're going to stop this old database, delete the container, upgrade the tag to the latest version again, then restart it.
```
docker stop postgres
docker rm postgres
~change docker-compose tag to "postgres:alpine"
docker-compose up
```

Now we can check if the container is running and ready to accept connections:
```
docker logs -f postgres
```

If all looks good, we'll copy over the `postgres-dump.sql` file to the container, and restore it:
```
docker cp postgres-dump.sql postgres:/
docker exec -it postgres sh
psql -U postgres < /postgres-dump.sql
```

Now exit from the container and restart the container, then watch the logs to ensure everything comes up as expected:
```
exit
docker restart postgres
docker logs -f postgres
```

All being well, everything will have gone well and you can bookmark this guide for the next major upgrade.

## SCRAM error

During my most recent upgrade (and subsequent check of this guide) I encountered an issue where the password encryption method was too old and the container wouldn't boot. To solve this, it's reasonably straightforward. You just need to rehash your password with the following:

```
docker exec -it postgres sh
psql -U postgres
\password yourexistingSQLpassword
exit
exit
docker restart postgres
```

* **Edit 2023-02-14:** Streamlined restoration instructions
* **Edit 2023-09-21:** SCRAM password encryption instructions added
