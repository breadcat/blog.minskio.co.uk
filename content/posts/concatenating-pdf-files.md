---
title: "Concatenating PDF files on Linux"
date: 2018-11-26 13:29:00
tags: ["Snippets", "Software", "Formats", "Linux"]
---

A simple request, with an equally simple solution. Firstly I tried using my old favourite `pandoc` which will apparently only output PDF files, not input them.

Some quick searching around lead me to `pdfunite` which did the job perfectly.

Simple issue the following command to merge 3 files into one final output file:
```
pdfunite page-1.pdf page-2.pdf page-3.pdf pages-out.pdf
```
Be careful when specifying your files here as the last file will be overwritten if it already exists.

If you don't have `pdfunite` installed, it's available via:
```
sudo apt-get install poppler-utils
```