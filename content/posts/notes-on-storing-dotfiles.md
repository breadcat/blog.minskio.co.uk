---
title: "Notes on storing Dotfiles"
date: 2020-06-28T15:29:00
lastmod: 2020-06-29T11:43:00
tags: ["Linux", "Selfhosted", "Software"]
---

I've run various versions of Linux on and off for quite a few years now, and I've never really thought about archiving my dotfiles in any meaningful way. I would re-install Linux so infrequently, and generally make so few changes that it never made sense to set up systems or repositories for config files.

Recently though, I reinstalled Linux on my laptop having to set up everything from scratch seemed like a real drag, with remembering what applications I had installed, little nuances in config files and keyboard layout modifications. So, I decided once and for all to archive what I was doing at the time to hopefully benefit somebody else in future.

Over the years, I've seen a few ways of going about this, but the one that looked most interesting to me was the so called "bare git repository", this appealed to me as I would already have git installed, it requires few changes to your workflow and you get the usual suite of versioning controls. Other interesting ways include extensive symlinks and GNU's stow utility.

So, without further ado, let's create a bare git repository. I sync a small-ish repository of files with all clients using `syncthing`, so it makes sense to keep this dotfile repository in this synchronised vault.

```
git init --bare $HOME/vault/src/dotfiles
```

Now, to interact with this new repository, we're going to add a new alias. Add the following to your `.bashrc` or which ever shell you're usings' equivalent:
```
alias dotfiles='git --git-dir=$HOME/vault/src/dotfiles --work-tree=$HOME'
```

Restart your shell or source your shells' \*rc file for this change to take effect.

What you'll want to run next is the following command to hide untracked files, allowing you to just add and commit the files you want.
```
dotfiles config --local status.showUntrackedFiles no
```

If you run `dotfiles status` now you'll see the repository is up to date, and in sync. If you'd like to add a dotfile to the repository, you can do so in the following way:
```
dotfiles add $HOME/.config/bspwm/bspwmrc
dotfiles commit -m "Added bspwm config file"
```

If you'd like to see what has changed before you commit them, you can run:
```
dotfiles diff $HOME/.config/bspwm/bspwmrc
```

Another point worth noting, [shamelessly stolen from jbauer](https://www.paritybit.ca/blog/how-i-manage-my-dotfiles) is omitting repository specific files (`readme.md`, `licence`, etc) via the `--skip-worktree` option, for example:
```
dotfiles add readme.md
dotfiles commit -m "Add Readme"
rm readme.md
dotfiles update-index --skip-worktree readme.md
```

And then to update this file:
```
dotfiles update-index --no-skip-worktree readme.md
dotfiles checkout -- readme.md
```

And you're done. This guide is by no means unique, but has been collated from many posts accross the Internet detailing this process.

* **Edit 2020-07-21:** Added note about diffing changes
* **Edit 2021-11-29:** Added note about removing files from the work tree