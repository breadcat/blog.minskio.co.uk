---
title: "Personal VIM cheatsheet"
date: 2020-06-14T12:58:00
tags : [ "Guides", "Learning", "Linux", "Servers", "Snippets", "Software", ]
---

When editing files on Linux, I've always used `nano`, it was always installed and `vi` just seemed incredibly awkward to use, with all the memes about never being able to exit, and weird things happening being right up my street. I'd tried `vimtutor` but was left in pretty much the same place as I started.

So, when the venerable [Luke Smith](https://lukesmith.xyz/) posted an [hour long walkthrough](https://www.youtube.com/watch?v=d8XtNXutVto) of his methods for completing vimtutor, I was hooked.

I've now moved all my Linux machines over to neovim and haven't looked back. Without further ado I present the notes cheatsheet that I made while watching the video, for future reference:

```
ZZ quit with saving
ZQ quit, without saving
zt makes current line the topmost
zz centre window around current line

A enters insert mode at the end of the line
I enters insert mode at the start of the line
a enters insert mode after current character
i enters insert mode before current character

Ctrl + r redo changes
Ctrl + g shows location status bar
gg go to the top line
G go to last line
25% move to 25% of the way through the file
'' move back to previous location before % movement
u undoes changes

v start visual selection/highlighting
Ctrl + v start visual selection as a block
v start visual selection/highlighting, using complete lines

gf open written filename in text, in vim

y yank/copy
yy yank whole line

. redo last command run

w move forwards word by word
b move backwards word by word

/ search for text going forwards
/ enter n next search result
/ enter N previous search result
? search for text going backwards
:set ic toggles case insensitivty
:set hlsearch highlights search results
:nohlsearch disables highlighted search results

r replace current letter with next letter you type
R replace mode, more than one character, but exact length matching

{ move cursor up 4 lines at a time
} move cursor down 4 lines at a time

v enters visual mode
V enters visual mode, whole lines

$ move to the end of the line
^ move to the start of the line
% jump to matching parenthesis

:s/old/new/ replace old with new once on a line
:s/old/new/g replace old with new every time on a line
:%s/old/new/g replace old with new every time in the whole file
:%s/old/new/gc replace old with new every time in the whole file, but prompting beforehand

:! run shell command
:r filename read text from filename
:r ! command read output from running command

:norm run command on highlighted lines

:setlocal spell! spelllang=en_gb start spellcheck
:setlocal spell! stop spellcheck
z= correct misspelled word when highlighted
]s jumps to next misspelled word

p put/paste previously deleted lines
5p paste, 5 times

x delete character under cursor
dw delete word (at the start of the word)
daw delete whole word including whitespace
diw delete whole word, excluding whitespace
di( delete everything inside parentheis
da( delete everything inside, and including parenthesis
dw delete word (current word, anywhere)
d$ delete the remainder of the line
D delete the remainder of the line
db delete one word backwards
d5w deletes 5 words forwards
dd deletes the whole line
3dd deletes the next 3 lines

c change mode, same as d, but goes to insert mode after
cw delete word, then enter insert mode
o insert new line and enter insert mode
O insert new line above current and enter insert mode

0 move to start of line
2w move 2 words to the left

:earlier 5m undo 5 minutes worth of changes
:later 5m redo 5 minutes worth of changes after undoing
```

One nice bonus that I picked up on from Lukes dotfiles was fixing line endings to the Linux format on every save which is just great.

Add the below to your `init.vim` file (if using neovim):

```
" Automatically deletes all trailing whitespace and newlines at end of file on save.
	autocmd BufWritePre * %s/\s\+$//e
	autocmd BufWritepre * %s/\n\+\%$//e
```

Another point worth adding is sometimes files formatted outside vim will contain '^@' symbols scattered between letters. These represent null characters and can be removed by running:
```
%s/[\x0]//g
```

* **Edit 2020-12-02:** Added notes regarding reading file contents and command output.
* **Edit 2021-01-02:** Added null character removal note.
