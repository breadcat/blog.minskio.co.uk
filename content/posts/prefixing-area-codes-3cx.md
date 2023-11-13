---
title: "Prefixing area codes on 3CX"
date: 2023-11-13T19:50:00
tags: ["Guides", "PBX", "Snippets", "Work"]
---

Just a quick guide on how to prefix an area code on outgoing calls on 3CX:


<ul><li>
Rule: 1<br>
Name: Local<br>
Prefix: 2-8<br>
Route 1: Provider, Prepend area code</li>
</ul>


<ul><li>
Rule: 2<br>
Name: National<br>
Prefix: 0-9<br>
Route 1: Provider</li>
</ul>

Takes care of 999, 111 and local area prefixes
