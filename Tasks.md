This is a revolving list of tasks that we need to do.  Priorities change constantly.

### Current Items ###
- Figure out why linux has a bandwidth allocation issue after a ^c from a program
- Get gr-osmosdr using async interface
- Get CLI using async interface
- Add device caching to bladeRF src/sink gr-osmosdr to allow for full-duplex support
- Verify operation with GQRX
- Flash recovery for bad flashes from CLI (https://github.com/Nuand/bladeRF/pull/96)
- CRC check FX3 firmware before running it, fallback to USB bootloader
- FPGA version number
- Si5338 MIMO/Expansion clock settings
- Si5338 fractional clock rate generation
- Create and simulate HDL models
- Create IQ correction block for HDL
- In-band scheduling support
- extio.dll support for Windows

### Future Items ###
- Create signal generator block for FPGA
- Create impulse latency RTT mode for FPGA
- Create counter mode for RX stream
- TX -> RX Loopback mode in HDL
- Store and load default FPGA image in SPI flash