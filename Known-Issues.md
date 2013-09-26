### FPGA Fails to load ###
There are a few different reasons for this.  One might be that the wrong size FPGA image is being used for the FPGA on your board.  Check to make sure you are using the 40kLE FPGA for x40 and the 115kLE FPGA for x115.

If you compile your library as Debug, and see it is still timing out, check `dmesg` to see if there is anything out of the ordinary - like no room on an ep ring.  This is a known issue in Ubuntu 12.04 with a 3.2 linux kernel.  The only known solution, so far, is to update to a newer kernel either through a distribution update or `sudo apt-get install linux-image-generic-lts-quantal`.

If you are using a Virtual Machine, there have been issues reported when changing the interfaces or claiming different interfaces.  The level of success has been varied depending on the type of port (XHCI or EHCI) and the VM software used.  We are currently working on native Windows support to help remedy the VM situation.


### libbladeRF Does Not Compile ###
If you see:

    backend/libusb.c:263:5: warning: implicit declaration of function ‘libusb_get_version’ [-Wimplicit-function-declaration]
    backend/libusb.c:264:5: error: dereferencing pointer to incomplete type

during compilation you have an old version of libusb and should probably upgrade! Get the latest version from http://libusbx.org/

### The serial number field is empty when doing a bladeRF-cli probe ###
This is a defect that will be fixed in the near future. The underlying issue is that a probe (using the libusb backend) attempts to gather information from a device via USB descriptors, and this serial is not yet reflected in those descriptors.

For the time being, this information can be queried via the bladeRF-cli's _version_ command, after a device has been opened.



### The following warning is printed ###
```
WARNING - [$SOME_PATH/libusb.c:818] FPGA currently does not have a version number.
```

The FPGA currently does not implement a version number, so 0's are returned. This is a TO DO item.

### Mac OSX Superspeed libusb does ns not show correct speed/crashes ###

If you've used ports to get libusb installed, there is a high likelihood that it is version 1.0.9.  Unfortunately, libusb didn't add superspeed support until version 1.0.15 for OSX.  We've had reports that the absolute latest version of libusb, 1.0.17, works on OSX.  Please update accordingly.

### GNU Radio doesn't work with both source and sink in the same flowgraph ###

Currently, both the source and sink try to open the device.  Unfortunately, only one can open it at a time.  The workaround is to understand the other block is trying to open an already opened device, and pass the already opened handle to the block instead of trying to open it again.

Work is under way to cache the opened handles and search through those first before trying to re-open the device.

### CLI gives a legacy mode warning ###

This warning comes up if you have a [firmware](FX3-Firmware) that is considered out-of-date and requires flashing.  Please update to the [latest firmware][latest] using the `bladeRF-flash` or `bladeRF-cli` tool.

[latest]: http://nuand.com/fx3/latest.img