---
title: "Removing large files from a Borg backup"
date: 2022-10-28T15:30:00
lastmod: 2023-02-28T18:13:00
tags: ["Linux", "Infrastructure", "Recovery", "Snippets", "Software", "Servers"]
---

I have a couple of 'backup systems'. For important documents that change frequently I use [Borg](https://www.borgbackup.org/), along with the helper script [borgmatic](https://torsion.org/borgmatic/) with my hosting being provided by [BorgBase](https://www.borgbase.com/). The three all tie together incredibly well and if the free tier of BorgBase didn't cover my every need I'd happily pay for it.

Occasionally however, I'll end up temporarily storing a large file in a directory that will be backed up on schedule which then increases my overall storage usage unnecessarily. The simple fix for this is just to delete the backups which contain the file, the more complicated option is to selectively remove just those files from a backup. I'll cover both cases in this post.

The guide assumes you already have a working Borgmatic setup.

# Removing a whole backup

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

# Removing single files from multiple backups

If in the above example you've run a few backups and don't want to entirely remove all backups containing these files, you can use Borgs' [recreate command](https://borgbackup.readthedocs.io/en/stable/usage/recreate.html) to rebuild your backups without certain patterns. You can find the details (and browse sizes/contents) using the commands covered above.

To list out backups that can be recreated, you issue:
```
borgmatic borg recreate --list
```

Once you know what you want to remove, you can perform a dry run using:
```
borgmatic borg recreate -e /full/path/to/file/you/wish/to.remove -n
```

If everything looks okay, remove the `-n` and issue the command again to recreate the backups excluding your pattern.

Once done, feel free view information and to compact the repo again.
```
borgmatic info
borgmatic compact
```

* **Edit 2023-02-28:** Added single file/multiple backups section