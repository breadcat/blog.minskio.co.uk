---
title: "GCZ, RVZ and WIA size comparison"
date: 2020-12-29T15:18:00
lastmod: 2025-07-30T12:13:00
layout: table
tags: ["Emulation", "Formats", "Games", "Media", "Software"]
---

For as long as I can remember, the [Dolphin emulator](https://dolphin-emu.org/) has supported GCZ compression, and since version 5.0-12188, it has added two new formats, WIA and RVZ (as outlined in [this blog post](https://dolphin-emu.org/blog/2020/07/05/dolphin-progress-report-may-and-june-2020/)). The original post has a quick overview of size comparisons, but it doesn't detail how well each compression method works.

I tried this with Kururin Squash!, a Japanese exclusive Gamecube game and sequel to the GBA exclusive Kuru Kuru Kururin. No junk data was removed during any of these tests.

The compressed sizes were as follows, with percentage values to match:

<table id="sortable-table">
  <thead><tr>
    <th>Format</th><th>Size (bytes)</th><th>Relative size (percent)</th>
  </tr></thead>
  <tbody>
	  <tr><td>ISO (Original)</td><td>1,459,978,240</td><td>610.57</td></tr>
	  <tr><td>ISO (Uncompressed)</td><td>239,116,400</td><td>100.00</td></tr>
	  <tr><td>GCZ (Standard)</td><td>107,272,770</td><td>44.87</td></tr>
	  <tr><td>WIA (No Compression)</td><td>239,149,168</td><td>100.01</td></tr>
	  <tr><td>WIA (Purge)</td><td>229,449,768</td><td>95.96</td></tr>
	  <tr><td>WIA (2MiB, bzip2, level 1)</td><td>106,515,768</td><td>44.55</td></tr>
	  <tr><td>WIA (2MiB, bzip2, level 5)</td><td>105,127,440</td><td>43.97</td></tr>
	  <tr><td>WIA (2MiB, bzip2, level 9)</td><td>104,732,188</td><td>43.79</td></tr>
	  <tr><td>WIA (2MiB, LZMA, level 1)</td><td>91,674,188</td><td>38.34</td></tr>
	  <tr><td>WIA (2MiB, LZMA, level 5)</td><td>86,715,744</td><td>36.26</td></tr>
	  <tr><td><b>WIA (2MiB, LZMA, level 9)</b></td><td><b>86,618,716</b></td><td><b>36.22</b></td></tr>
	  <tr><td>WIA (2MiB, LZMA2, level 1)</td><td>91,688,008</td><td>38.34</td></tr>
	  <tr><td>WIA (2MiB, LZMA2, level 5)</td><td>86,728,708</td><td>36.27</td></tr>
	  <tr><td>WIA (2MiB, LZMA2, level 9)</td><td>86,728,708</td><td>36.27</td></tr>
	  <tr><td>RVZ (128KiB, bzip2, level 1)</td><td>106,771,616</td><td>44.66</td></tr>
	  <tr><td>RVZ (128KiB, bzip2, level 5)</td><td>106,339,216</td><td>44.47</td></tr>
	  <tr><td>RVZ (128KiB, bzip2, level 9)</td><td>106,339,216</td><td>44.47</td></tr>
	  <tr><td>RVZ (512KiB, bzip2, level 1)</td><td>106,568,640</td><td>44.57</td></tr>
	  <tr><td>RVZ (512KiB, bzip2, level 5)</td><td>105,239,600</td><td>44.01</td></tr>
	  <tr><td>RVZ (512KiB, bzip2, level 9)</td><td>105,093,188</td><td>43.95</td></tr>
	  <tr><td>RVZ (2MiB, bzip2, level 1)</td><td>106,515,768</td><td>44.55</td></tr>
	  <tr><td>RVZ (2MiB, bzip2, level 5)</td><td>105,127,440</td><td>43.96</td></tr>
	  <tr><td>RVZ (2MiB, bzip2, level 9)</td><td>104,732,188</td><td>43.80</td></tr>
	  <tr><td>RVZ (128KiB, LZMA, level 1)</td><td>96,654,712</td><td>40.42</td></tr>
	  <tr><td>RVZ (128KiB, LZMA, level 5)</td><td>92,597,268</td><td>38.72</td></tr>
	  <tr><td>RVZ (128KiB, LZMA, level 9)</td><td>92,521,648</td><td>38.69</td></tr>
	  <tr><td>RVZ (512KiB, LZMA, level 1)</td><td>93,891,916</td><td>39.27</td></tr>
	  <tr><td>RVZ (512KiB, LZMA, level 5)</td><td>89,668,640</td><td>37.50</td></tr>
	  <tr><td>RVZ (512KiB, LZMA, level 9)</td><td>89,586,320</td><td>37.47</td></tr>
	  <tr><td>RVZ (2MiB, LZMA, level 1)</td><td>91,674,188</td><td>38.34</td></tr>
	  <tr><td>RVZ (2MiB, LZMA, level 5)</td><td>86,715,744</td><td>36.27</td></tr>
	  <tr><td><b>RVZ (2MiB, LZMA, level 9)</b></td><td><b>86,618,716</b></td><td><b>36.22</b></td></tr>
	  <tr><td>RVZ (128KiB, LZMA2, level 1)</td><td>96,666,280</td><td>40.43</td></tr>
	  <tr><td>RVZ (128KiB, LZMA2, level 5)</td><td>92,608,300</td><td>38.73</td></tr>
	  <tr><td>RVZ (128KiB, LZMA2, level 9)</td><td>92,532,604</td><td>38.70</td></tr>
	  <tr><td>RVZ (512KiB, LZMA2, level 1)</td><td>93,905,124</td><td>39.27</td></tr>
	  <tr><td>RVZ (512KiB, LZMA2, level 5)</td><td>89,681,068</td><td>37.51</td></tr>
	  <tr><td>RVZ (512KiB, LZMA2, level 9)</td><td>89,598,776</td><td>37.47</td></tr>
	  <tr><td>RVZ (2MiB, LZMA2, level 1)</td><td>91,688,008</td><td>38.34</td></tr>
	  <tr><td>RVZ (2MiB, LZMA2, level 5)</td><td>86,728,708</td><td>36.27</td></tr>
	  <tr><td>RVZ (2MiB, LZMA2, level 9)</td><td>86,631,708</td><td>36.23</td></tr>
	  <tr><td>RVZ (128KiB, Zstandard, level 1)</td><td>111,340,684</td><td>46.57</td></tr>
	  <tr><td>RVZ (128KiB, Zstandard, level 5)</td><td>105,730,496</td><td>44.22</td></tr>
	  <tr><td>RVZ (128KiB, Zstandard, level 10)</td><td>104,140,564</td><td>43.56</td></tr>
	  <tr><td>RVZ (128KiB, Zstandard, level 22)</td><td>100,217,436</td><td>41.91</td></tr>
	  <tr><td>RVZ (512KiB, Zstandard, level 1)</td><td>109,719,948</td><td>45.89</td></tr>
	  <tr><td>RVZ (512KiB, Zstandard, level 5)</td><td>103,921,924</td><td>43.46</td></tr>
	  <tr><td>RVZ (512KiB, Zstandard, level 10)</td><td>102,535,204</td><td>42.88</td></tr>
	  <tr><td>RVZ (512KiB, Zstandard, level 22)</td><td>97,270,064</td><td>40.68</td></tr>
	  <tr><td>RVZ (2MiB, Zstandard, level 1)</td><td>108,380,880</td><td>45.33</td></tr>
	  <tr><td>RVZ (2MiB, Zstandard, level 5)</td><td>100,285,556</td><td>41.94</td></tr>
	  <tr><td>RVZ (2MiB, Zstandard, level 10)</td><td>98,800,396</td><td>41.32</td></tr>
	  <tr><td>RVZ (2MiB, Zstandard, level 22)</td><td>93,857,212</td><td>39.25</td></tr>
</tbody>
</table>

As you can see, from this extremely limited test, the best performing archive format is LZMA using a 2MiB block size at compression level 9. It doesn't matter which container, out of RVZ and WIA you use.
