# Upgrading on Linux #

1. Download and install all of the bladeRF libraries and utilities as described here, https://github.com/Nuand/bladeRF/wiki/Linux-startup
2. Download the latest FX3 image and flash it:
<pre>wget http://nuand.com/fx3/latest.img ; bladeRF -f latest.img</pre>

### Troubleshooting ideas for Linux ###

1. Some xHCIs have problems identifying bladeRFs running older firmware versions. Repeating the upgrade instructions with the bladeRF plugged into a USB2.0 port should make it visible to the OS.
2. Try forcing into Cypress Bootloader (see below for instructions) and use bladeRF-flash to upgrade the device.

# Upgrading on Windows #
**Coming soon: Youtube video**

Pre-compiled bladeRF utilities can be installed from http://nuand.com/downloads/bladerf_win_installer.exe
or they can built from source following the instructions at https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Windows .

### Method #1: bladeRF-cli ###
1. Acquire bladeRF-cli and bladeRF libraries either from the installer or source code (see previous paragraph)
2. Download the latest FX3 image and flash it:
<pre>wget http://nuand.com/fx3/latest.img ; bladeRF -f latest.img</pre>
3. Unplug and plug the bladeRF

### Method #2 : bladerf_winflasher ###
1. Optionally plug the bladeRF into a USB2.0 port. In case only USB3.0 ports exist, use a USB2.0 micro cable.
2. Download and run http://nuand.com/downloads/bladerf_win_installer.exe
3. Check the "Yes, upgrade bladeRF firmware" checkbox<pre>![Yes, upgrade bladeRF firmware](http://nuand.com/upgrade.png)</pre>
4. Finish running the installer
5. Unplug and plug the bladeRF

### Troubleshooting ideas for Windows ###

1. Some xHCIs have problems identifying bladeRFs running older firmware versions. Repeating the upgrade instructions with the bladeRF plugged into a USB2.0 port should make it visible to the OS.

2. Follow the "Force Cypress Bootloader" instruction below. Once the jumper is removed you can use the Cypress USB control center utility from the SDK to SPI flash the device. The FX3 SDK that installs with the Cypress USB control center can be downloaded from http://www.cypress.com/?rID=57990 .

# Force Cypress Bootloader #
This is the foolproof way of flashing your bladeRF on Windows by forcing the unit to boot to the Cypress bootloader.

1. First you have to take one of the power jumpers, it doesn't matter which, and short the two outer pins on J64 (shown below). This causes the FX3 to not see the SPI flash and boot into the cypress bootloader.
![J64](http://nuand.com/J64.png)

2. Plug the device in at this point.

3. **IMPORTANT:** Once the device is plugged in, and recognized as a Cypress bootloader remove the jumper from J64.