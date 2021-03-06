---
title: "Formatting dumped subtitles into a vocabulary list"
date: 2020-05-28T16:52:00
lastmod: 2020-07-05T17:59:00
tags : [ "Formats", "Languages", "Linux", "Media", "Snippets", "Software", ]
---

As per my previous post, you should now have a single `srt` subtitle file, to convert this into a single word list that you can begin translating away at, you can run the below verbose script.

```
tr ' ' '\n' < subs.srt | \
	sed -e 's/<[^>]*>//g' | \
	tr '[:upper:]' '[:lower:]' | \
	tr -d '\>\/!-.:?,.\",[:digit:]' | \
	tr -d '…' | \
	sed -e '/^[[:space:]]*$/d' -re 's/\s+$//' -re 's/\{...\}//' | \
	sort -u > subs-sort.srt
```

In short, this will break all spaces into new lines, remove HTML tags, make everything lowercase, remove some strange characters and empty lines then finally sort the list while removing duplicates.

One issue I've noticed is some _special_ characters won't be converted to lowercase &Aring; to &aring; for example. I don't have an automated workaround for you aside from specifying the letters individually for example using:

<pre><code>tr '&AElig;&Oslash;&Aring;&Auml;&Ouml;&ETH;&THORN;&Aacute;&Eacute;&Iacute;&Oacute;&Uacute;&Yacute;' '&aelig;&oslash;&aring;&auml;&ouml;&eth;&thorn;&aacute;&eacute;&iacute;&oacute;&uacute;&yacute;'</pre></code>

* **Edit 2020-09-23:** Added elipses removal, fixed pipes
* **Edit 2020-07-05:** Added {\an} tag removal
