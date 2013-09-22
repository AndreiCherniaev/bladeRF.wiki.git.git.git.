_This is a work in progress -- below are just some initial notes_
 
# Obtaining git for Windows #
1. Download and install [msysgit](http://code.google.com/p/msysgit/downloads/list?can=2&q=%22Full+installer+for+official+Git+for+Windows%22) If you plan to submit patches to the bladeRF project, please select the _Checkout as-is, commit Unix-style line endings_ option in the installer.
2. Download and install [tortoisegit](http://code.google.com/p/tortoisegit/wiki/Download)

For more information, see the [mysysgit](https://github.com/msysgit/msysgit/wiki/InstallMSysGit) and [tortoisegit](http://code.google.com/p/tortoisegit/wiki/SetupHowTo) wiki pages about their install procedures.

# Installing Visual Studio 2012 Express Desktop #
1. Download [Visual Studio 2012 Express Desktop](http://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-windows-desktop) from Microsoft.
2. Follow the installation instructions, including any post-install updates.

**NOTE**: Visual Studio 2012 Express Desktop corresponds to Visual Studio 11 in CMake.

# Installing libusbx #

1. Download the latest Windows binary release of libusbx, which also include development headers: https://sourceforge.net/projects/libusbx/files/releases/1.0.17/binaries/
2. Extract the contents to a location of your choice. Make note of this location so that you can later provide it to CMake. The default configuration assumes that files will be in ```C:/Program Files (x86)/libusbx-1.0.17``` If you wish to change the directory, edit ```\host\cmake\modules\FindLibUSB.cmake```
3. Get the device driver installer (zadig): http://sourceforge.net/projects/libwdi/files/zadig/
4. Open Zadig. 
5. Go to Device->Create New Device. 
6. Type a device name (i.e., "bladeRF") in the text box. In the driver spinbox, select libusbK. Specify the VID/PID (`1d50`/`6066`) in the USB ID fields.
7. Plug the device into the computer and open Device Manager.  A new device called `bladeRF` should show up with a yellow bang next to it in device manager.
8. Right-click on the `bladeRF` entry and select "Update Driver Software...".  Choose "Browse my computer for driver software", then "Let me pick from a list of device drivers on my computer".  Click "Have Disk..." and point it to the location that Zadig installed the driver to (C:\usb_driver).  Select "bladeRF" and continue through the wizard.
9. Device Manager should now show `bladeRF` under `libusbK USB Devices`.

# Installing pthreads-win32 #
The pthreads library is required to build a few tests and utilities. A few steps are required to install this pthreads implementation. See [the pthreads-win32 website](http://www.sourceware.org/pthreads-win32/) for more information.

1. Download the latest release. Currently this is [version 2.9.1](ftp://sourceware.org/pub/pthreads-win32/pthreads-w32-2-9-1-release.zip).
2. Extract the contents of the release zip.
3. Copy the contents of the _Pre-built.2_ directory to _C:\Program Files (x86)\pthreads-win32_

# CMake #
1. Download and install CMake for Windows: http://www.cmake.org/cmake/resources/software.html
2. Run the CMake GUI utility. 
3. Under "Where is the source code", browse to ```some_dir/bladeRF/host```. 
4. Create a new directory, ```some_dir/bladeRF/host/build```. 
5. Under "Where to build the binaries", browse to the newly created /bladeRF/host/build. Click the Configure button. 
6. Select your appropriate version of Visual Studio.  Current development is done using "Visual Studio 11" or "Visual Studio 11 Win64" as the generator (Visual Studio 2012).  Select "Use default native compilers", then click "Finish". 
7. If the configuration fails, double check the values for `LIBUSB_PATH` and `LIBPTHREADSWIN32_PATH`, and re-run the configuration.
8. Click on the Generate button.
9. A visual studio solution should now be available, ```bladeRF.sln``` 

# Visual Studio #
1. CMake has created a `bladeRF.sln` file.  Open Visual Studio, and open this file.
2. The following projects should show up in the Solution Explorer.
    - ALL_BUILD
    - bladeRF-cli
    - INSTALL
    - libbladerf_shared
    - libbladerf_test_async
    - uninstall
    - ZERO_CHECK
3. ...

# Verifying USB Functionality #
Using the [SuperSpeed USB Hardware/Software tools][ssusbtools].

[ssusbtools]: http://www.usb.org/developers/ssusb/ssusbtools/