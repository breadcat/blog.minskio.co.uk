---
title: "Ericsson-LG PBX notes"
date: 2021-05-21T13:48:00
lastmod: 2023-03-21T13:13:00
tags: ["Guides", "Hardware", "PBX", "Snippets", "Work"]
---

With the news that Ericsson LG will be discontinuing the eMG80 PBX, I'm publishing some notes I made years ago with the hopes that someone finds them useful. I'll publish similar notes for other manufacturers at later dates.

# Hardware

## DIP switches
* 1 and 2 off in normal operation
* 2 on during boot defaults the system

## Port wiring
* Analogue extension RJ11: Pins 3 and 4 (innermost 2)
* Digital extension RJ11: Pins 2 and 5 (nextmost 2)
* Line cord RJ45: Line 1, Pins 1 and 2. Line 2 Pins 5 and 6
* Digital extension RJ45: Pins 4 and 5 (innermost 2)
* BT>RJ45 line cord conversion: Pins 2 and 5 > Pins 4 and 5 (innermost 2)

## Tollring modified switch ports
* Port 1 - PBX
* Ports 2-3 - Voice modules
* Port 7 - Network uplink
* Port 8 - Call recording server

# Software

## Default details
The default IP address of the system on first boot is `10.10.10.2`. The default login details are `admin : 1234`.

## Network ports
* 1720 TCP/UDP: Site to site linking
* 443 TCP: Default web interface (changeable via PGM160)
* 5060 UDP: SIP signalling
* 5588 TCP/UDP: IP phone signalling
* 6000-6047 UDP: VOIM voice channels
* 7000-7015 UDP: Voice channels
* 7100-7115 UDP: Voice channels
* 7300-7315 UDP: Voice channels
* 7878 TCP: Click to call
* 8000-8047 UDP: VOIM voice channels
* 8899 TCP: UCS client
* 9000-9047 UDP: VOIM voice channels

## Music on hold filenames
File format specifics [described here](/ffmpeg-audio-conversions-for-pbx/).
* eMG80: 71.wav
* UCP100: 201.wav

## COS lockdown steps
* PGM 160 > Remote VM Access: Off
* PGM 160 > VM Notify: Disable
* PGM 160 > Remove VM Forward Access: Off
* PGM 127 > 101 to *last extension number* > VSF Access: Disable
* PGM 224 > Deny A Table > 09, 00, 1, #, *, 070
* PGM 226 > 999, 911, 912, 111, 112

## IP authenticated SIP trunk setup
Assuming remote setup, using an IP phone for testing
* PGM 102 > Firewall IP Address - *wanipaddress*
* PGM 113 > *ipphonestation* - Station CLI 1 - DDI number minus leading zero
* PGM 132 > 13/2401 - Firewall IP Address & Router IP Address - Copied from PGM102
* PGM 133 > *siptrunknumbers* - Domain & Proxy Server Address - *sipserveraddress*, Domain Acceptance - Domain Only
* PGM 140 > *siptrunknumbers* - CO Type - DDI, CO/IP Group - 2
* PGM 145 > *siptrunknumbers* - DID Conversion Type - Modify Using Flexible DID Conversion Table
* PGM 151 > *siptrunknumbers* - CLIP Table Index & COLP Table Index - Station CLI
* PGM 160 > CO Line Choice - ROUND
* PGM 231 > *ddilast3digits* - *ipphonestation*

* For CLIR: PGM 133 > Privacy(CLIR) Presentation - Privacy: id & Anonymous & P-Preferred-ID

## LCR prefix area code
* PGM 220 > LCR Access Mode: Loop & Direct CO LCR
* PGM 221 > Index 1 - Range 0-7 - ICR Type: COL, 010101 Start at 2 through to 9
* PGM 222 > Index 1 - Area code digits (i.e. 01282)

# Usability
* Call group pickup key: 115 > Programming (numbering plan) > 566
* Call log flex key: PGM115 > programming (PGM code) > 57
* Change date/time from attendant phone: Trans/PGM 0 4 1
* Clear attendant alarm: 565
* Day/Night toggle flex key: PGM 115 > Programming (PGM Code) > 8*612
* Two way record flex key: PGM 115 > Programming (PGM Code) > 80
* Don't repeat auto-attendant message: PGM 160 > DISA Retry Count > 0
* Setting an unconditional divert flex key via the web interface: PGM 115 > Programming (Numbering Plan) > 5546 speed bin number
* Toggle CO line isolation: Trans/PGM 0 7 2
* Calls cut off after 10 minutes: PGM180 > Unsupervised Conference Timer
* ATD Off Duty / Unavailable: 562


## Updates
* **2022-05-23:** Added two way record instruction
* **2023-03-21:** Added modified switch port setup
