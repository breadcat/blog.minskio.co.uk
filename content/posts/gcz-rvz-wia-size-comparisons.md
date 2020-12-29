---
title: "GCZ, RVZ and WIA size comparison"
date: 2020-12-29T15:18:00
tags: ["Emulation", "Formats", "Games", "Media", "Software"]
---

For as long as I can remember, the [Dolphin emulator](https://dolphin-emu.org/) has supported GCZ compression, and since version 5.0-12188, it has added two new formats, WIA and RVZ (as outlined in [this blog post](https://dolphin-emu.org/blog/2020/07/05/dolphin-progress-report-may-and-june-2020/)). The original post has a quick overview of size comparisons, but it doesn't detail how well each compression method works.

I tried this with Kururin Squash!, a Japanese exclusive Gamecube game and sequel to the GBA exclusive Kuru Kuru Kururin. No junk data was removed during any of these tests.

The compressed sizes were as follows, with percentage values to match:

| Format | Size (bytes) | Relative size (percent) |
| --- | --- | --- |
| ISO (Original) | 1,459,978,240 | 610.57 |
| ISO (Uncompressed) | 239,116,400 | 100.00 |
| GCZ (Standard) | 107,272,770 | 44.87 |
| WIA (No Compression) | 239,149,168 | 100.01 |
| WIA (Purge) | 229,449,768 | 95.96 |
| WIA (2MiB, bzip2, level 1) | 106,515,768 | 44.55 |
| WIA (2MiB, bzip2, level 5) | 105,127,440 | 43.97 |
| WIA (2MiB, bzip2, level 9) | 104,732,188 | 43.79 |
| WIA (2MiB, LZMA, level 1) | 91,674,188 | 38.34 |
| WIA (2MiB, LZMA, level 5) | 86,715,744 | 36.26 |
| **WIA (2MiB, LZMA, level 9)** | **86,618,716** | **36.22** |
| WIA (2MiB, LZMA2, level 1) | 91,688,008 | 38.34 |
| WIA (2MiB, LZMA2, level 5) | 86,728,708 | 36.27 |
| WIA (2MiB, LZMA2, level 9) | 86,728,708 | 36.27 |
| RVZ (128KiB, bzip2, level 1) | 106,771,616 | 44.66 |
| RVZ (128KiB, bzip2, level 5) | 106,339,216 | 44.47 |
| RVZ (128KiB, bzip2, level 9) | 106,339,216 | 44.47 |
| RVZ (512KiB, bzip2, level 1) | 106,568,640 | 44.57 |
| RVZ (512KiB, bzip2, level 5) | 105,239,600 | 44.01 |
| RVZ (512KiB, bzip2, level 9) | 105,093,188 | 43.95 |
| RVZ (2MiB, bzip2, level 1) | 106,515,768 | 44.55 |
| RVZ (2MiB, bzip2, level 5) | 105,127,440 | 43.96 |
| RVZ (2MiB, bzip2, level 9) | 104,732,188 | 43.80 |
| RVZ (128KiB, LZMA, level 1) | 96,654,712 | 40.42 |
| RVZ (128KiB, LZMA, level 5) | 92,597,268 | 38.72 |
| RVZ (128KiB, LZMA, level 9) | 92,521,648 | 38.69 |
| RVZ (512KiB, LZMA, level 1) | 93,891,916 | 39.27 |
| RVZ (512KiB, LZMA, level 5) | 89,668,640 | 37.50 |
| RVZ (512KiB, LZMA, level 9) | 89,586,320 | 37.47 |
| RVZ (2MiB, LZMA, level 1) | 91,674,188 | 38.34 |
| RVZ (2MiB, LZMA, level 5) | 86,715,744 | 36.27 |
| **RVZ (2MiB, LZMA, level 9)** | **86,618,716** | **36.22** |
| RVZ (128KiB, LZMA2, level 1) | 96,666,280 | 40.43 |
| RVZ (128KiB, LZMA2, level 5) | 92,608,300 | 38.73 |
| RVZ (128KiB, LZMA2, level 9) | 92,532,604 | 38.70 |
| RVZ (512KiB, LZMA2, level 1) | 93,905,124 | 39.27 |
| RVZ (512KiB, LZMA2, level 5) | 89,681,068 | 37.51 |
| RVZ (512KiB, LZMA2, level 9) | 89,598,776 | 37.47 |
| RVZ (2MiB, LZMA2, level 1) | 91,688,008 | 38.34 |
| RVZ (2MiB, LZMA2, level 5) | 86,728,708 | 36.27 |
| RVZ (2MiB, LZMA2, level 9) | 86,631,708 | 36.23 |
| RVZ (128KiB, Zstandard, level 1) | 111,340,684 | 46.57 |
| RVZ (128KiB, Zstandard, level 5) | 105,730,496 | 44.22 |
| RVZ (128KiB, Zstandard, level 10) | 104,140,564 | 43.56 |
| RVZ (128KiB, Zstandard, level 22) | 100,217,436 | 41.91 |
| RVZ (512KiB, Zstandard, level 1) | 109,719,948 | 45.89 |
| RVZ (512KiB, Zstandard, level 5) | 103,921,924 | 43.46 |
| RVZ (512KiB, Zstandard, level 10) | 102,535,204 | 42.88 |
| RVZ (512KiB, Zstandard, level 22) | 97,270,064 | 40.68 |
| RVZ (2MiB, Zstandard, level 1) | 108,380,880 | 45.33 |
| RVZ (2MiB, Zstandard, level 5) | 100,285,556 | 41.94 |
| RVZ (2MiB, Zstandard, level 10) | 98,800,396 | 41.32 |
| RVZ (2MiB, Zstandard, level 22) | 93,857,212 | 39.25 |

As you can see, from this extremely limited test, the best performing archive format is LZMA using a 2MiB block size at compression level 9. It doesn't matter which container, our of RVZ and WIA you use.
