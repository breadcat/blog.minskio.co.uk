---
title: "Removing ACSM DRM from E-Books"
date: 2018-11-24T10:22:00
tags: ["Books", "Guides", "Software", "Windows"]
---
Recently I found a rare-ish e-book available via the Internet Archive's [Open Library](https://openlibrary.org/) project, there was a waiting list and I wanted to read this on my Kindle without needlessly tying it up for other people wanting to read it.
The combination of these two circumstances leads me to stripping the DRM from this file so I could work with it how I wanted.

Firstly, you'll need to install [version 2.0.1 of Adobe Digital Editions](https://www.adobe.com/support/digitaleditions/downloads.html) as newer versions use a newer encryption scheme. With this installed, authorise your computer and open your `acsm` file to verify everything's correct with the book.

Next, you'll need to install [Calibre](https://calibre-ebook.com/) which you should probably already have installed if you're working with Kindles in any real manner.

Next, you should grab the latest release of the excellent [De-DRM Tools](https://github.com/apprenticeharper/DeDRM_tools/releases) by Apprentice Harper. With this downloaded, extract the contents of the file, open Calibre and add the plugin via Preferences > Plug-ins > Load plug-in from file. The file you want is `DeDRM_plugin.zip`. Restart Calibre and the plugin should be installed.

Lastly, grab the plain `epub` file from your `%userprofile%\documents\My Digital Editions` directory. Import this into Calibre by dragging it into the main window and you're done. A file you can work with at long last. Now return that book so other people can read it.