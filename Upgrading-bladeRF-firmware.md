This page describes how to update the firmware on the bladeRF.

Upgrading from v1.0 is a little trickier than upgrading from more recent firmware images. If you received your unit before September 10th, you might encounter some issues on Windows; the troubleshooting ideas below should provide enough methods to flash any v1.0 bladeRF.

# Upgrading on Linux #

1. Ensure you've installed all libraries and utilities, per the ''Getting Started'' guides
2. Download the latest FX3 image and flash it:
<pre>wget http://nuand.com/fx3/latest.img && bladeRF -f latest.img</pre>

## Troubleshooting ideas for Linux ##

1. Some xHCIs have problems identifying bladeRFs running older firmware versions. Repeating the upgrade instructions with the bladeRF plugged into a USB2.0 port should make it visible to the OS.
2. Try forcing into Cypress Bootloader (see below for instructions) and use bladeRF-flash to upgrade the device.

# Upgrading on Windows #
[YouTube video HOWTO explaining how to force bladeRF into the Cypress FX3 bootloader](http://youtu.be/oolb9e_9qTc)

Pre-compiled bladeRF utilities can be installed from http://nuand.com/downloads/bladerf_win_installer.exe
or they can built from source following the instructions at https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Windows .

## Method #1: bladeRF-cli ##
1. Acquire bladeRF-cli and bladeRF libraries either from the installer or source code (see previous paragraph)
2. Download the latest FX3 image and flash it:
<pre>wget http://nuand.com/fx3/latest.img ; bladeRF -f latest.img</pre>
3. Unplug and plug the bladeRF

## Method #2 : bladerf_winflasher ##
bladerf_winflasher uses CyUSB so the unsigned driver is required for it to work. The flasher can run during the installer, but it can be run directly from <pre>C:\Program Files (x86)\bladeRF\bladerf_winflasher.exe</pre>.

1. Optionally plug the bladeRF into a USB2.0 port. In case only USB3.0 ports exist, use a USB2.0 micro cable.
2. Download and run http://nuand.com/downloads/bladerf_win_installer.exe
3. Check the "Yes, upgrade bladeRF firmware" checkbox<pre>![Yes, upgrade bladeRF firmware](http://nuand.com/upgrade.png)</pre>
4. Finish running the installer
5. Unplug and plug the bladeRF


## Troubleshooting ideas for Windows ##

1. Some xHCIs have problems identifying bladeRFs running older firmware versions. Repeating the upgrade instructions with the bladeRF plugged into a USB2.0 port should make it visible to the OS.

2. Follow the "Force Cypress Bootloader" instruction below. Once the jumper is removed you can use the Cypress USB control center utility from the SDK to SPI flash the device. The FX3 SDK that installs with the Cypress USB control center can be downloaded from http://www.cypress.com/?rID=57990 .

# Force Cypress Bootloader #
This is the foolproof way of flashing your bladeRF by forcing the unit to boot to the Cypress bootloader.

1. First you have to take one of the power jumpers, it doesn't matter which, and short the two outer pins on J64 (shown below). This causes the FX3 to not see the SPI flash and boot into the cypress bootloader.
![J64](http://nuand.com/J64.png)

2. Plug the device in at this point.

3. **IMPORTANT:** Once the device is plugged in, and recognized as a Cypress bootloader remove the jumper from J64. If you forget to remove the jumper, the device will not be able to write to the SPI flash.


# Getting further help
If you're stuck in the bootloader or having trouble flashing the image, don't fret! Join the #bladerf channel on Freenode or visit the [http://nuand.com/forums/viewforum.php?f=4 Nuand troubleshooting forum]. There's generally a number of folks around who have experience with flashing the device.