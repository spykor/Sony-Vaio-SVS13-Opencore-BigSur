# Opencore for Sony Vaio SVS13A1S9ES updated for Big Sur with BCM94352HMB wifi support


**Specifications**

Sony Vaio SVS13A1S9ES 13.3″

CPU i7-3520M @ 2.90GHz Ivy Bridge

Mem 6GB

Display 1600×900

Graphics card Intel HD Graphics 4000

Ethernet card Realtek RTL8168E-VL/8111E-VL PCI Express Gigabit Ethernet

WiFi/BT card Azurewave AW-CE123H half mini PCIe card


**Background**

Everything was working fine until I tried to upgrade from Catalina 10.15.7 to Big Sur 11.1 (20C69), which is the highest version of macOS this type of CPU (Ivy Bridge) supports.


The other thing is that while Bluetooth works fine (WiFi/BT combo half mini card), wifi does not work and the system does not detect at all the wifi part. as I said everything was workin fine on Catalina.


Opencore version that works right is 0.6.5! Everything above that report system incompatibility.

**Prerequisites**

Upgrade all related items (kexts for Broadcom chips and wifi) to the latest versions (AirtportBrcmFixup.kext, BrcmBluetoothInjector.kext, BrcmFirmwareData.kext, BrcmPatchRAM3.kext, FakePCIID_Broadcom_WiFi.kext, FakePCIID.kext, etc) and while I was at it upgrade other kexts too (VirtualSMC.kext, Lily.kext, WhateverGreen.kext, etc). 


**Solution for fixing no wifi**

Download IO80211Catalina.kext from https://github.com/khronokernel/IO80211-Patches and inject it by using the Opencore config.plist.

Mount EFI partition of the system disk using ESP mounter app. Copy IO80211Catalina.kext to the EFI>OC>Kexts section. Leave the standard AirtportBrcmFixup.kext in which includes both plugins.

Open config.plist using ProperTree editor and take an **OC Snapshot** (command + R) to include the new kext and save the file again. Close the application.
In case system reboots before reaching the apple logo, boot from another source, mount EFI partition, load **config.plist** with ProperTree and set **AirPortBrcm4360_Injector.kext** to **"false"** in the Kernel>Add section. Also you might set **MaxKernel** to 19.9.9. also.

Eject the EFI partition and reboot. Clear the NVRAM once just in case.

**Important note**

For someone who is initilally installing the macOS to Vaio SVS13 laptop, he/she needs to do some serious DSDT and SSDT patching. Follow the instructions from this article to do that https://www.insanelymac.com/forum/topic/309549-el-capitan-uefi-clover-on-sony-vaio-s/

**What works:**

built-in keyboard (with brightness keys)

built-in trackpad (multi gestures)

HDMI video/audio with hotplug

native WiFi via BCM94352HMB

Bluetooth (with handoff) using BCM94352HMB

native USB3

native audio with AppleHDA

built-in mic

built-in camera

native power management

battery status

sleep

accelerated graphics for HD4000

wired Ethernet

**What doesn't work**

Brightness control


