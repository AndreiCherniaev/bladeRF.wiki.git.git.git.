## FPGA Fails to load ##
There are a few different reasons for this.  One might be that the wrong size FPGA image is being used for the FPGA on your board.  Check to make sure you are using the 40kLE FPGA for x40 and the 115kLE FPGA for x115.

If you compile your library as Debug, and see it is still timing out, check `dmesg` to see if there is anything out of the ordinary - like no room on an ep ring.  This is a known issue in Ubuntu 12.04 with a 3.2 linux kernel.  The only known solution, so far, is to update to a newer kernel either through a distribution update or `sudo apt-get install linux-image-generic-lts-quantal`.

If you are using a Virtual Machine, there have been issues reported when changing the interfaces or claiming different interfaces.  The level of success has been varied depending on the type of port (XHCI or EHCI) and the VM software used.  We are currently working on native Windows support to help remedy the VM situation.