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
6. Type a device name (i.e., "bladeRF") in the text box. In the driver spinbox, select libusbK. Specify the VID/PID (`1d50`/`6066`) in the USB ID fields.
7. Plug the device into the computer and open Device Manager.  A new device called `bladeRF` should show up with a yellow bang next to it in device manager.
8. Right-click on the `bladeRF` entry and select "Update Driver Software...".  Choose "Browse my computer for driver software", then "Let me pick from a list of device drivers on my computer".  Click "Have Disk..." and point it to the location that Zadig installed the driver to (C:\usb_driver).  Select "bladeRF" and continue through the wizard.
9. Device Manager should now show `bladeRF` under `libusbK USB Devices`.

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
12. A visual studio solution should now be available, ```bladeRF.sln``` 


