---
title: "Recoving de-duplicated git repositories"
date: 2022-05-05T15:37:00
tags: ["Linux", "Recovery", "Snippets", "Software"]
---

I have a number of software projects in a single directory, using `git` for version control which is shared across most of my computers using `syncthing`.

Late one night, I wanted to de-duplicate a folder full of files I'd been working on over the course of a few weeks. I'd work through a directory then as part of my 'completing' the work, would de-duplicate it using `jdupes` to ensure no files were duplicated. I'm sure you can see where this is going.

As part of my workflow, I was navigating directories, tapping the up arrow to repeat my last command and hitting enter with reckless abandon. I noted the command took a little longer than usual and immediately realised what I'd done. I went to assess the damage, when browsing using git, cgit, or even the github desktop client, all my repositories were broken except for a single bare repository.

The first thing to do is to find any potentially affected directories. This can simply be done via `find . -type d -name '.git'`. As what was deleted will only ever be duplicated content, you should be able to rebuild these repositories quite easily with the following:

```
cd affected-project
mv ".git" ".gitcontents"
git init
rsync --remove-source-files -av ".gitcontents" ".git"
find ".gitcontents" -empty -delete
```

Which should get you up and working again.

The moral of the story is to keep a backup, and then make a backup of your backup (just in case).