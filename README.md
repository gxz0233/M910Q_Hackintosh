# M910Q_Hackintosh
## Lenovo M910Q tiny 
This M910Q bought 2nd hand from ebay with :
* Intel(R) Core(TM) i5-7500T CPU @ 2.70GHz   2.71 GHz, Kaby Lake
* HYNIX 8G DDR4
* SAMSUNG 250G SSD

Upgrading with:
* SK Hynix 8GB DDR4 1Rx8 PC4-2400T SO-DIMM MEMORY RAM MODULE HMA81GS6AFR8N-UH
* Hackintosh WiFi Card 1200Mbps BCM94360NG DW1560 BCM94360CD macOS PC Adapter BT4
![](Docs/Images/5.png)
## Hackintosh steps
### Opencore
* Install Python on Windows
* Download [Opencore](https://github.com/acidanthera/OpenCorePkg/releases), I am using release 0.7.4 here.
### Mac OS
* Mac OS big sur 11.6. 
* Make a bootable USB disk follow [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html)
### ACPI SSDT
M910Q only need one customized SSDT-PLUG.aml in ACPI directory, it created for each computer. Tools:
* SSDTTime (Windows)
### config.plist
Follow   [ OpenCore Install Guide for Desktop Kaby Lake ](https://dortania.github.io/OpenCore-Install-Guide/config.plist/kaby-lake.htmlhttp://google.com). make your own config.plist, the steps are not hard at all. 
Tools:
* ProperTree(Windows)
* tools for updating config.plist on USB disk, there is so many tools out there, I used DiskGenius(Windows)
### Drivers and Kexts
* Drivers
  * HfsPlus.efi
  * OpenCanopy.efi
  * OpenRuntime.efi

* Kexts
  * AppleALC.kext
  * Innie.kext
  * IntelMausi.kext
  * Lilu.kext
  * NVMeFix.kext
  * SMCProcessor.kext
  * SMCSuperIO.kext
  * USBToolBox.kext (for USB ports mapping, created later)
  * UTBMap.kext (for USB ports mapping, created later)
  * VirtualSMC.kext
  * WhateverGreen.kext
  * XHCI-unsupported.kext
### BIOS
* Turn off securiy boot
* USB
  * Turn on Legacy support
  * Turn on XHCI support( I forgot exact name, but should not matter)
### Hard disk partation and EFI transfer
I install dual boot on one SSD disk. This M910Q come with a Windows 10 installed on one partation assigned to C:, the partation table looks like the table below, a recovery partation about 1G at the tail of the disk but I don't care:  

      EFI (200 M) | partation for c: (>200G)

use Windows disk manage tools to shrink C: partation, make room for two new partations:  new EFI and MacOS. Modified partations look like:

      EFI (200 M) | partation for c: (>100G) | EFI_new(200M) | MacOS (~70G)

Then use EFI recovery/transfer tool make a Windows EFI on EFI_new, delete old EFI, copy Opencore EFI to old EFI, the idea is use Opencore EFI to replace Windows EFI so computer will boot Opencore EFI, Opencore EFI is able to find Windows EFI( stored in EFI_new) and boot it for you. this how dual boot works.  
There are many ways for doing dual boot, I just pick the dummy method. 
### Install Mac OS
The installation process may hung at some places, twick USB BIOS settings should solve the issue.  
After these steps, the M910Q has Windows and MacOS installed. 
![](Docs/Images/4.png)
### USB ports mapping
After some version of big sur, MacOS can not support more than 15 USB ports, so USB mapping is necessary if your motherboard has more than 15 USB ports. For M910Q, Bluetooth is not functioning because MacOS couldn't find the right USB ports for the card. use this Windows tool: [USBToolBox](https://github.com/USBToolBox/tool/releases) to create two Kexts, then put them in Kexts directory. 

  * USBToolBox.kext
  * UTBMap.kext
These two Kexts are only thing you need for M910Q. Bluetooth should work after a reboot. 
![](Docs/Images/1.png)
![](Docs/Images/Inked2_LI.jpg)
![](Docs/Images/3.png)
## Remaining issues
Sleep not working, it can not recovery after a long sleep, you have to hard reset the computer. 
