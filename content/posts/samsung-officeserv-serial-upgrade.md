---
title: "Upgrading a Samsung OfficeServ PBX using the serial interface"
date: 2019-08-27T13:55:00
tags: ["guides", "hardware", "linux", "pbx", "projects", "software", "work"]
---

# Requirements:

* COM Serial port
* Serial to CD Audio cable
* [Putty](https://www.putty.org/)
* Firmware package `ap30v501.pkm cs30v493.pkm dr30v501.pkm ms30v125.pkm rd30v501.pkm rt30v501.pkm ws30v501.pkm`
* BootROM `bt30v493.bin` file
* Voicemail Package `VM_L_UK.tgz`
* [TFTPD server](http://tftpd32.jounin.net/tftpd32_download.html)
* OfficeServ Device Manager `osdm.exe`

# System Setup

## Cards
The only cards you'll need is something to connect up a digital phone to, so either a 4DM or a 2DM card in the middle slot to connect up a phone.

## DIP Switches
To set the system into UK mode, your DIP switches must be set to `01000000`. For other country codes, look into the documentation.

## Cable Setup
I'm using an old IDE CD-ROM drive audio cable to serial that I've created for this purpose. As there aren't 3 contiguous pins, I used both ends of the cable to give me 2 + 1 for the 3 pins.

The serial port on the PBX is the 3 pins behind the line card slot at the bottom of the system. I'm using a StarTech USB Serial adapter on Windows 10 and it's functions perfectly.

# Computer Setup

## Putty Setup
* Serial Line > COM1
* Speed > 38400
* Connection Type > Serial
* Connection > Serial > Flow control > None

## Tftpd32 Setup
All you want to alter in this is the Current Directory so that it offers all your firmware and bootrom files. The Server interface should be your computers local IP address and TFTP security should be set to None.

# Upgrade Process

Fire up the system with your serial cable connected and you should see the system booting up. You'll need to press the Q key to interrupt the boot process.

## MAC Address
If you're replacing en existing cabinet, you can change the current system's MAC address to match the old system ensuring you can use the same licences. For example:
```
setenv eth1addr 00:16:32:82:9c:8b ; setenv ethaddr 00:16:32:82:9c:8b ; saveenv
```

## Boot ROM

Once you've booted up, enter `printenv` to check current boot variables. If your `bootfile` variable is lower than `v493` you will need to upgrade this via:
```
setenv bootfile bt30v493.bin ; setenv ipaddr 10.0.0.247 ; setenv gatewayip 10.0.0.1 ; setenv serverip 10.0.0.66 ; setenv netmask 255.255.255.0 ; saveenv ; run updateboot; run clearenv ; reset
```

The important values to look at here are `ipaddr` which is your OfficeServ system's IP address, and `serverip` which is your TFTPD server address. By default, the system's IP address is 10.0.2.10 and the gateway address is 10.0.2.1, but you'll likely want to change these.

## System Firmware

Once you're on bootrom v4.93 you can upgrade the rest of the system using:
```
setenv ipaddr 10.0.0.247 ; setenv gatewayip 10.0.0.1 ; setenv serverip 10.0.0.66 ; setenv netmask 255.0.0.0 ; setenv mspname ms30v125.pkm ; setenv cspname cs30v493.pkm ; setenv rfsname rd30v501.pkm ; saveenv ; run nanderaseall ; run install
```
This process will take a long time to complete, but you'll be given the status  of the upgrade via your serial connection.


# First boot
Once booted up, verify your firmware version via: `Trans 800 4321 1 Speaker 727`, this should be 5.01. Log out with `Trans 800 0 Trans` and connect up using OSDM.

## Login
The address will be whatever you set the `ipaddr` variable to in the preceeding steps. The default password for firmware v5.01 is `#PBX1357sec.com`. It will ask you to change this when you first login.

## Licences
Once logged in, if you have existing licences, add them via MMC860.
<!-- * Resource: `GYNGHELH-PLJSOOWP-THUVZIU9-F1A0FRCM-7UDVFS3O-7Z5CHYEQ`-->
<!-- * SIP Stack: `GSWVEHM2-2MZLLNBC-SUGBLGM7-2U9FBL6X-ORS3O2AW-7QOXALMT`-->
<!-- * Service: `NMWPHHM2-2MZLLNBC-SUUXQGM7-UU97BMIX-OJS3O2AW-7QOXALMT`-->

## Voicemail
Lastly, as we've entirely wiped this system during the upgrade process you'll need to upload your voicemail prompt package, usually called `VM_L_UK.tgz`. This can be done via:
```
File Control > Program > File > VM_L_UK.tgz > Upload
```
After the file has been uploaded, reboot the system in MMC811. Once the system is back up and working, dial the VM group (usually 509) to ensure your prompts are all working. Enjoy your new system.
