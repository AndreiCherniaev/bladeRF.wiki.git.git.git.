_This is a work in progress -- below are just some initial notes_
 
# Obtaining git for Windows #
1. Download and install [msysgit](http://code.google.com/p/msysgit/downloads/list?can=2&q=%22Full+installer+for+official+Git+for+Windows%22)
2. Download and install [tortoisegit](http://code.google.com/p/tortoisegit/wiki/Download)

For more information, see the [mysysgit](https://github.com/msysgit/msysgit/wiki/InstallMSysGit) and [tortoisegit](http://code.google.com/p/tortoisegit/wiki/SetupHowTo) wiki pages about their install procedures.

# Installing libusbx #

1. Download the latest Windows binary release of libusbx, which also include development headers: https://sourceforge.net/projects/libusbx/files/releases/1.0.16/binaries/
2. Extract the contents to a location of your choice. Make note of this location so that you can later provide it to CMake. The default configuration assumes that files will be in ```C:/Program Files (x86)/libusbx-1.0.16```
3. Get the device driver installer (zadig): http://sourceforge.net/projects/libwdi/files/zadig/
4. Open Zadig. 
5. Go to Device->Create New Device. 
6. Type a device name (i.e., "bladeRF") in the text box. In the driver spinbox, select libusbK. Specify the VID/PID in the USB ID fields. 
7. In the button dropdown, select "Install Driver". Click on the "Install Driver" button. Drivers should be deployed to C:\usb_drivers.
8. To replace the Windows USB Composite Device driver, go to device manager and right-click on the bladeRF USB Composite Device (it will likely be the only one under "Universal Serial Bus Controllers" with a bang icon on it; otherwise, look for a USB device with the expected VID/PID by inspecting the detailed properties). _(Is this still required with the latest firmware changes?)_ 
9. Select "Update Driver Software...". Choose "Browse my computer for driver software", then "Let me pick from a list of device drivers on my computer". The following screen should display both the "bladeRF" driver created by Zadig and the "USB Composite Device" driver provided by Windows (and currently in use by the device). Select "bladeRF" and continue through the wizard.

# Visual Studio #
**_TO DO_**

# CMake #
1. Download and install CMake for Windows: http://www.cmake.org/cmake/resources/software.html
2. Run the CMake GUI utility. 
3. Under "Where is the source code", browse to ```some_dir/bladeRF/host```. 
4. Create a new directory, ```some_dir/bladeRF/host/build```. 
5. Under "Where to build the binaries", browse to the newly created /bladeRF/host/build. Click the Configure button. 
6. Select "Visual Studio 11" as the generator and "Use default native compilers", then click "Finish". 
7. The configuration will fail due to the lack of libtecla and an possibly libusb. 
8. Uncheck ENABLE_INTERACTIVE_TECLA 
9. If needed, fill out the correct location for ```LIBUSB_PATH```
10. Re-run Configuration. It should now succeed.
11. Click on the Generate button.
12. A visual studio solution should now be available, ```Project.sln``` _(Anyone know how to get CMake to rename this?)_


