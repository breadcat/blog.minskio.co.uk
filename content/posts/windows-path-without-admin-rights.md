---
title: "Windows PATH without admin rights"
date: 2025-08-20T16:37:00
tags: ["Snippets", "Windows", "Work"]
---

I was recently given a new (albeit more locked down) work laptop and wanted to amend my `PATH` variable to use some common command line tools I usually end up installing.

My usual process is to fire up `sysdm.cpl`, and hit up "Environmental Variables" under the "Advanced" tab and edit it from there, however this now requires admin permissions, and providing these amends the admin users path, not your current user.

The workaround here is to run `rundll32 sysdm.cpl,EditEnvironmentVariables` which will get you directly to the page you're after. Amend away!