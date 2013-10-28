This is a revolving list of tasks that we need to do.  Priorities change constantly.

### Current Items ###
- Audit library messages being printed out to make sure they are of the appropriate type (debug, info, etc)
- Clean up lms.c (return values, style, etc.)
- Complete CLI scripting
- Figure out why linux has a bandwidth allocation issue after a ^c from a program
- Get a Windows 8 machine and see why accesses fail
- Add some hotkey support to test_repeater to allow the user to adjust RX/TX gains
- Developing test and release cycle plans on the wiki. Needs to cover versioning & tagging schemes, branch usage, and tests required to pass before version release **at a minimum**. litghost as opened and issue and provided [some initial ideas.](https://github.com/Nuand/bladeRF/issues/105)
- Create coding style & patch/pull request guidelines document
- Create a BUGS/Getting Help document detailing information to gather and provide when posting issues to the forum or IRC
- Get FPGA building with Quartus 13.0sp1
    - Seems to work out of the box with warnings from Qsys regarding versioning of components
- Make UART run at 4608000 baud rate instead of 115200
    - Make the NIOS run at 16*4608000 clock rate and set the divisor appropriately
- FPGA version number
- In-band scheduling support
- Fix or remove FS USB descriptors.
- extio.dll support for Windows

### Future Items ###
- Si5338 MIMO/Expansion clock settings
- Automatic IQ Calibration
- Create IQ correction block for HDL
- Create and simulate HDL models
- Create signal generator block for FPGA
- Create impulse latency RTT mode for FPGA
- Create counter mode for RX stream
- TX -> RX Loopback mode in HDL
- Store and load default FPGA image in SPI flash