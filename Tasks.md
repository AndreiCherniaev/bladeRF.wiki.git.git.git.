This is a revolving list of tasks that we need to do.  Priorities change constantly.

### Current Items ###

**Unassigned**
- Figure out why linux has a bandwidth allocation issue after a ^c from a program
- Get a Windows 8 machine and see why accesses fail
- In-band scheduling support
- Fix or remove FS USB descriptors.
- Flash-related failures in Windows
- extio.dll support for Windows

**bpadalino**
- Get FPGA building with Quartus 13.0sp1
    - Seems to work out of the box with warnings from Qsys regarding versioning of components
- Make UART run at 4608000 baud rate instead of 115200
    - Make the NIOS run at 16*4608000 clock rate and set the divisor appropriately
- FPGA version number
- Automatic IQ Calibration
- Create IQ correction block for HDL
- Create and simulate HDL models
- Create signal generator block for FPGA
- Create counter mode for RX stream
   - This is currently in the dev-uart_speedup branch

**jynik**
- Clean up lms.c (return values, style, etc.) and fix bugs in loopback modes
- Finish up dev-libuse_cmake_cleanup
- start()/stop() in gr-osmosdr support for bladeRF (horizon may be working on this...keeping in touch with him on this)
- Address unimplemented functions the CLI
- Remove synchronous interface from libbladeRF, develop an auxiliary lib that presents a sync i/f using the libbladerf async interface
- Developing test and release cycle plans on the wiki. Needs to cover versioning & tagging schemes, branch usage, and tests required to pass before version release **at a minimum**. litghost as opened and issue and provided [some initial ideas.](https://github.com/Nuand/bladeRF/issues/105)
- Create coding style & patch/pull request guidelines document
- Create a BUGS/Getting Help document detailing information to gather and provide when posting issues to the forum or IRC
- rpi sweeping spectrum analyzer demo based upon fosphor
    - Pending on FPGA FFT block
- Add some hotkey support to test_repeater to allow the user to adjust RX/TX gains

### Future Items ###
- Si5338 MIMO/Expansion clock settings
- Create impulse latency RTT mode for FPGA
- TX -> RX Loopback mode in HDL
- Speed up FPGA autoloading (and flashing of FPGA image)
    - There's some delays that can replaced with quicker device polls