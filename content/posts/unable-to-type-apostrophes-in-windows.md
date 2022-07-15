---
title: "Unable to type apostrophes in Windows"
date: 2022-07-15T10:53:00
tags: ["Snippets", "Software", "Windows"]
---

Randomly one day, my keyboard just straight up stopped entering the apostrophe character. No real reason why, Windows gonna Windows I guess.

Anyway, after trying to clean the contacts, unplug and replug the keyboard, reboot, try another keyboard, I gathered it has to be something in software. I'm not one to shy away from remapping keys, but quitting [my program](https://github.com/breadcat/ahk-assistant) also made no difference.

I eventually narrowed it down to it being assigned as a dead key used for shortcuts to swap between languages. The fix was mercifully pretty simple (once you know where to look).

* Windows + R
* `intl.cpl`
* Formats tab
* Language preferences link
* Language page
* Keyboard button in the top 5 icons
* Language bar options link
* Advanced Key Settings tab
* Change Key Sequence button
* Switch Input Language > Not Assigned
* Switch Keyboard Language > Not Assigned
* OK