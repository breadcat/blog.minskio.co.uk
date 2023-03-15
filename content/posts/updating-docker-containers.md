---
title: "Upgrading docker containers"
date: 2023-02-14T22:35:00
lastmod: 2023-03-15T17:22:00
tags: ["Docker", "Guides", "Linux", "Servers", "Software"]
---

I moved all my server infrastructure some ~8 months ago to a simpler arm64 stack hosted on Oracles free tier. As part of this (and to reduce some headaches) I purposely chose not to constantly update my docker containers. The amount of times things would mess up, versus get fixed was just not worth the hassle. Eventually though, you do need to update things which brings me to todays post.

We're going to be using [watchtower](https://containrrr.dev/watchtower/) for everything here, it's multi-arch, comes as a docker container and will gracefully handle environmental variables which I'm using in my stack. We're exposing `docker.sock` so just be aware of the litany of security issues this could entail.

First we'll list out what can be updated:
```
docker run -it -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --monitor-only --run-once
```

If you're only after updating a few containers, you can [read up on the arguments](https://containrrr.dev/watchtower/arguments/) for narrowing the scope of updates.
We're a bit reckless though (and made plenty of backups) so we're updating everything all at once:
```
docker run -it -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once
```

After you've updated everything, you can remove any old and unused containers with:
```
docker image prune -f
docker system prune -af
```

...now if you'll excuse me, I need to go read through my [updating postgres blog post](/upgrading-postgresql-in-an-alpine-docker-container/) again...

* **Edit 2023-03-15:** Improved empty directory deletion section