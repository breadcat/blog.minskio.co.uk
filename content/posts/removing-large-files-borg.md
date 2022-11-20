---
title: "Removing large files from a Borg backup"
date: 2022-10-28T15:30:00
tags: ["Linux", "Infrastructure", "Recovery", "Snippets", "Software", "Servers"]
---

I have a couple of 'backup systems'. For important documents that change frequently I use [Borg](https://www.borgbackup.org/), along with the helper script [borgmatic](https://torsion.org/borgmatic/) with my hosting being provided by [BorgBase](https://www.borgbase.com/). The three all tie together incredibly well and if the free tier of BorgBase didn't cover my every need I'd happily pay for it.

Occasionally however, I'll end up temporarily storing a large file in a directory that will be backed up on schedule which then increases my overall storage usage unnecessarily. The simple fix for this is just to delete the backups which contain the file.

The guide assumes you already have a working Borgmatic setup.

Firstly, you can find the details of your repository using:
```
borgmatic info
borgmatic rlist
```

If you don't remember the ID or date, you can narrow down the point when the large file got added by mounting the backup repository and searching by size using something like `ncdu`:
```
mkdir borg_mount
borgmatic mount --mount-point borg_mount/
ncdu borg_mount
```

You'll want to unmount the repository before you try to delete anything, otherwise you'll get a lock error. This can be done via `umount borg_mount`.

Once you know the ID (or ID's) affected, you can issue the [arbitary delete command](https://torsion.org/borgmatic/docs/how-to/run-arbitrary-borg-commands/), for example:
```
borgmatic borg delete host-dateTtime
```

Once this has been done, you can compact your repository, then you should be done.
```
borgmatic compact
```