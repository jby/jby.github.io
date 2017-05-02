---
layout: post
title: Install Fedora 25 on Intel® Rapid Storage Technology
---

So, to avoid having more people running into this strange problem, I'd thought I'd share my experience.

One of my users tried to upgrade his workstation ( a <a href="http://www.dell.com/us/business/p/optiplex-7040-desktop/pd">Dell Optiplex 7040</a>, small form factor) from Fedora 23 to Fedora 25, this seemed to go well - up until the point where he rebooted after runnning the actual upgrade.

The system refused to find the disk where the system was installed.

After a couple of tries, he reverted to re-installing from USB.

If the system was set to use UEFI-boot he could choose the "Fedora" system disk as boot-device, it would not boot though. It booted as far as mounting the root-disk, timing out a couple of times and then saying "Unable find device /dev/fedora/root".

If we reset the system to use Legacy-boot mode, the BIOS would not allow the hard drive to be chosen for boot-device, it could be selected manually from the BIOS boot menu (accessed by pressing F12) though.

Within the BIOS setup you can chose SATA operational mode, which by default is set to use the Intel® Rapid Storage Technology - if this is changed to AHCI all is back to full working mode. We could set the "Fedora" system disk as boot device (using UEFI boot mode) and it would boot up and function without problems.
