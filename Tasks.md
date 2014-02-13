This is a revolving list of tasks that we need to do.  Priorities change constantly.

### Current Items ###

**Unassigned**
- Figure out why linux has a bandwidth allocation issue after a ^c from a program
- Get a Windows 8 machine and see why accesses fail
- In-band scheduling support
- Discipline VCTCXO using 1pps from GPSDO
- Discipline VCTCXO using 10MHz external reference
- Parse NMEA messages from GPS in FPGA for timestamping
- Add ability to 'reset on next 1pps' signal for TX/RX sample synchronization
- Fix or remove FS USB descriptors.
- Flash-related failures in Windows
- extio.dll support for Windows

**bpadalino**
- Automatic IQ Calibration
- Create and simulate HDL models
- Create signal generator block for FPGA
- Create RRC filter for FPGA to push ATSC filtering burden

**jynik**
- Address unimplemented functions the CLI
- Remove synchronous interface from libbladeRF, develop an auxiliary lib that presents a sync i/f using the libbladerf async interface (**In progress in dev-libbladerf_sync**)
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