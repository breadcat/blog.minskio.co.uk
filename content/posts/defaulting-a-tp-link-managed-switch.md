---
title: "Defaulting a TP-Link managed switch"
date: 2025-12-10T15:43:00
tags: ["Snippets", "Work"]
---

At work I recently came across a locked down TP-Link managed switch. I could access the password interface using a web browser but the password had already been set and there's no reset button on the device itself.

No matter, a bit of searching and I found how to default these using the console port and a serial cable.

Firstly you're going to want to find your COM address, then connect to it with the following options:

* Speed: 38400
* Data: 8 bit
* Parity: none
* Stop bits: 1 bit
* Flow control: none

Power on the switch while connected and hammer your [ENTER] key until you see a `tplink>` prompt. Enter `2` as your option, then `Y` to confirm device reset.

Once the device reboots you should have a defaulted switch where you can now login.