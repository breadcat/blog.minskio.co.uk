---
title: "Working around the NY Times paywall in Firefox"
date: 2021-06-16T09:53:00
tags: ["Formats", "Linux", "Snippets", "Windows"]
---

I was recently linked to an [interesting article](https://www.nytimes.com/1997/01/21/science/seismic-mystery-in-australia-quake-meteor-or-nuclear-blast.html) on the New York Times website. However on trying to view this I was immediately blocked by the paywall despite this being the first article I'd read there.

Not to be deterred by this and not wanting to wipe any browsing history or caches I investigated the cause. As you'd expect the cause of this (and all of lifes problems) is JavaScript. To work around this, you can disable JavaScript temporarily to read the article:

* Browse to your article, receive the paywall notification

* Open the developer tools, using the F12 key, or by selecting the 'Inspect' context menu entry.

* Go to the Debugger tab and click the cog icon in the top right.

* Select 'Disable JavaScript' and the page will reload, allowing you to read the article.