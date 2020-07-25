---
title: "Installing Resynthesizer for GIMP on Windows"
date: 2020-07-25T02:43:00
tags: ["Guides", "Snippets", "Software", "Windows"]
---
I had an image that I wanted to remove a section from in a content aware manor, much like what is available in Photoshop. For such a simple edit, I opted to use [GIMP](https://www.gimp.org/) instead of pirating photoshop, but the plugin required manually installing.

Windows binary can be grabbed from [this archived registry repo](https://github.com/pixlsus/registry.gimp.org_static/raw/master/registry.gimp.org/files/Resynthesizer_v1.0-i686.zip), once downloaded you want to extract the contents of the `Resynthesizer_v1.0-i686\` directory inside the archive into your `%appdata%\GIMP\2.10\plug-ins` directory.

You'll likely want to restart GIMP for these additional files to take effect.

Once restarted, you can load your image and make your desired selection. To use the plugin, browse to `Filters > Enhance > Heal Selection...`
