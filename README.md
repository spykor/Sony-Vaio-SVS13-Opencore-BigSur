# Opencore for Sony Vaio SVS13A1S9ES updated for Big Sur with BCM94352HMB wifi support and Brightness control (AppleBacklightFixup.kext)


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

Mount EFI partition of the system disk using ESP mounter app. Copy **IO80211Catalina.kext** to the EFI>OC>Kexts section. Leave the standard AirtportBrcmFixup.kext in which includes both plugins.

Open **config.plist** using **ProperTree** editor and take an **OC Snapshot** (command + R) to include the new kext and save the file again. Set **AirPortBrcm4360_Injector.kext** to **"false"** in the Kernel>Add section. Also you might set **MaxKernel** to 19.9.9. Close the application.

Eject the EFI partition and reboot. Clear the NVRAM once just in case.

**Fixing Brightness Control**

For an unknown reason Big Sur broke screen brightness. 
After succesfully installing Big Sur onto Sony Vaio SVS13, controlling Screen Brightness was not possible anymore, either by using the function keys (Pause/Break = Up, Fn+Delete/ScrLK = Down) that were perfectly working on Catalina, or by using the slide in the System Preferences > Displays.

To fix the problem follow the instructions from this tutorial: https://www.youtube.com/watch?v=UDev1FdhUr8
or this https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightfixup-kext.218222/

Download the appropriate files from the tutorial's links and place them in the OC appropeiate directories (ACPI, Kexts) and edit the config.plist.

The tricky part is to **remove the Brightness fix patch done to the DSDT** that was working up to Catalina, because the fix for Big Sur requires the removal of the DSDT patch.

Here are the steps:

Copy the patched DSDT from EFI>OC>ACPI to Desktop.

Keep a copy of it for backup reasons

Disassemble it by using **iasl -da -dl DSDT.aml**

Open the dsl file with **MaciASL** and locate the Brightness patch (Device (PNLF)) and mark down the lines range of the patch.

Open the dsl file with an editor and delete the lines of the patch and save the file.

Open the edited file with MaciASL again, compile the file and see if everuthing is OK.

If it's OK, execute "Save as..." and save the file as Machine Language file (aml).

Place the newly compiled DSDT.aml into EFI>OC>ACPI, together with the other files of the Brightness fix (kexts, etc).

Open the config.plist with ProperTree editor, execute a Snapshot (Command+R) and save the new config.plist.

Unmount the ESP partition (EFI) and reboot the system

You should now have a working Brighness control.

**Important note**

For someone who is initilally installing the macOS to Vaio SVS13 laptop, he/she needs to do some serious DSDT and SSDT patching. Follow the instructions from this article to do that https://www.insanelymac.com/forum/topic/309549-el-capitan-uefi-clover-on-sony-vaio-s/

**What works:**

built-in keyboard

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

**Brightness control works after new patching**



