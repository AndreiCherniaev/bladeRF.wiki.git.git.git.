The FX3 firmware has undergone some extensive development in the last two months and has rapidly progressed.  While the development continues, this should help to serve as a progress report of history and give a preview for future direction.

| Version | MD5                                |Description |
| :-----: | :--------------------------------: |:---------- |
| [1.4]   | `44d169733c623dd0ffe6ad9c2b74b2db` | Changed the alternate interface settings such that altsetting 0 is now a NULL interface and the FPGA config altsetting is now altsetting 3. |
| [1.3]   | `6f5e42e52c84f7c237aebc5ae6138a55` | Refactored the SPI flash code and give back return codes on success or failure so host side debugging of failures is easier. |
| [1.2]   | `92406eb0905510f590769d0d063e76a6` | Moved from 3 separate interfaces to a single interface with 3 altsettings where altsetting 0 is FPGA config, altsetting 1 is RF Link and altsetting 2 is SPI flash writing. |
| [1.1]   | `d26d1b1d4bc212c407107c4c6e7740d8` | Added calibration and OTP SPI flash reading. |
| [1.0]   | `5b8b60eaf3fff3f7d1a446ac07ec75c3` | Initial shipping code.  This code setup 3 separate interfaces - interface 0 being the FPGA config, interface 1 being the RF link and interface 2 being SPI flash writing. |

The most up-to-date firmware is always a convenient [latest] URL.

[latest]: http://nuand.com/fx3/latest.img

[1.4]: http://nuand.com/fx3/1.4.img
[1.3]: http://nuand.com/fx3/1.3.img
[1.2]: http://nuand.com/fx3/1.2.img
[1.1]: http://nuand.com/fx3/1.1.img
[1.0]: http://nuand.com/fx3/1.0.img

## How to flash ##

Please see the `bladeRF-flash` tool or the `bladeRF-cli` on how to flash a new image of the firmware to your device.

## Current Architecture ##

The FX3 is the main connection between the host PC and the bladeRF device itself.  This connection handles:

- Configuring the FPGA
- Shuffling samples for RF TX/RX
- Firmware updates to the on-board SPI flash
- Control of the RF and other components through the FPGA via a UART link

This is handled by using alternate interface settings in the FX3 firmware.  Each alternate setting handles a specific operating condition.  Since the operating conditions are somewhat orthogonal, it can be seen that the device gets put into these different modes for long periods of time and doesn't cycle through them randomly.

| Alt. Interface Setting | Name | Description |
| :--------------------: | :--- | :---------- |
| 0                      | NULL | No real operation here.  The device is mostly sitting idle and is just in a known state which doesn't really have any functional purpose. |
| 1                      | RF Link | Bulk endpoint interfaces for transmit and receive samples to go over the bus.  A separate set of bulk interfaces are used for communication with the peripherals hanging off the FPGA over the UART.  These peripherals include the trim DAC for the VCTCXO, Si5338 clock generator and LMS6002D FPRF transceiver. |
| 2                      | SPI flash | Used for updating and writing the firmware of the device. |
| 3                      | FPGA Config | Used for sending down an FPGA RBF file and loading the FPGA. |

## Future Direction ##

There has always been a desire to run the device in a headless mode so all that is required is power and the device loads the FX3 firmware from SPI, then loads the FPGA from SPI, and is fully autonomous.  To achieve this, a few things have to happen:

- Storage and loading of FPGA images in SPI flash
- Headless FPGA image

Currently, the FX3 runs the GPIF in 32-bit mode which only leaves the I2S and UART peripherals readily available to be used.  Without getting in the way of samples and using the GPIF to communicate back and forth with the FPGA, the UART was used as a side channel to the sample flow.  Unfortunately this limits us to a maximum of 4Mbps for the connection speed.  Since it is mainly configuration or setup of registers internal to the FPGA or to peripherals hanging off the FPGA, it isn't necessary to go very fast.  As of now, the UART is only running at 115.2kbps instead of the maximum 4Mbps.