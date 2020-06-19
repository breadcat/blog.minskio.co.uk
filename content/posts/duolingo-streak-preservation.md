---
title: "Duolingo Streak Preservation"
date: 2019-01-16T09:51:00
lastmod: 2020-06-19T00:33
tags: ["Languages", "Linux", "Servers", "Snippets", "Software"]
---

I'm a great fan of Duolingo, and while I have a number of issues with it I still consider it a generally useful platform for scratching the itch of learning a language.
One incentive they employ is to have a daily streak, indicating that you've maintained your practice for a certain number of days without slipping up once.

Now, for better or for worse, this streak is now something I take a humble amount of pride in. Some may consider it unsporting  whereas others may consider it out and out cheating, but once you've amassed a certain number of lingots the streak can be maintained near indefinitely.

The project we're using to achieve this is the comprehensive [Unofficial Duolingo API](https://github.com/KartikTalwar/Duolingo).

Firstly, you want to clone this project into a folder, and then start a new text file in the same directory with the following contents:
```
#!/usr/bin/env python3

import duolingo
lingo = duolingo.Duolingo('username', password='password')
lingo.buy_item('streak_freeze', 'nb')
```
The only values here than need altering are the username, password and language abbreviation code (nb for Norwegian Bokmål)

### Cron
Then you'll want to run the script regularly via cron which can be done by adding the below to your crontab with `crontab -e`, I run the above script a few times a day via cron just to make sure it runs. My line is as follows:
```
5 */8 * * * $HOME/bin/duolingo/auto.py
```
Where `auto.py` is the script you saved above. This will run the script every 8 hours (00:01, 08:01 and 16:01).

### Docker
In the past, I've [created a docker file](https://github.com/breadcat/Dockerfiles/commit/7ee24ece87d6604a5af373d334cfddf5713a11d5#diff-97f0aec7ad82d5d15585c03bdbd65392) for this, but I wouldn't recommend using it via docker as failures are generally more silent via docker.


* **Edit 2020-06-19:** Personally, I've [recently moved this](https://github.com/breadcat/Dockerfiles/commit/d56a650fd02d33061af4e7b1c4e85ce0812b4a5b) into a function in a larger project, however this is currently broken due to [an upstream bug](https://github.com/KartikTalwar/Duolingo/issues/79)