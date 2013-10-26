This page contains information pertaining to [FX3 firmware](http://www.nuand.com/fx3) development

# Current Architecture #

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

# Future Direction #

There has always been a desire to run the device in a headless mode so all that is required is power and the device loads the FX3 firmware from SPI, then loads the FPGA from SPI, and is fully autonomous.  To achieve this, a few things have to happen:

- Storage and loading of FPGA images in SPI flash
- Headless FPGA image

Currently, the FX3 runs the GPIF in 32-bit mode which only leaves the I2S and UART peripherals readily available to be used.  Without getting in the way of samples and using the GPIF to communicate back and forth with the FPGA, the UART was used as a side channel to the sample flow.  Unfortunately this limits us to a maximum of 4Mbps for the connection speed.  Since it is mainly configuration or setup of registers internal to the FPGA or to peripherals hanging off the FPGA, it isn't necessary to go very fast.  As of now, the UART is only running at 115.2kbps instead of the maximum 4Mbps.


# Verifying USB Functionality #
* [SuperSpeed USB Hardware/Software tools][ssusbtools].

[ssusbtools]: http://www.usb.org/developers/ssusb/ssusbtools/

# Open tasks #
There are several issues in the FX3 firmware that should be resolved. 

 - UART errors are not detected (framing/overflow)
 - SPI errors are not detected (underflow/overflow)
 - Once UART errors are detected, no way to report counters.  Need to develop diagnostic data interface.
 - It would be really nice to have logging support in the FX3 firmware.
 - Fatal errors in the FX3 firmware result in lockup.  Better to some how report to host problem occured.
 - Implement suspend/resume (low priority)
 - Add git rev to fx3 firmware build (https://github.com/Nuand/bladeRF/tree/dev-fx3_version_string)

Open questions:
 - Order of shutting down EPs and DMAs.  Should halt EP then DMA, or DMA then EP.  Or either works?
 - DMA for RF data is byte oriented.  Should be symbol oriented?
 - Unclear if endpoint halt feature is implemented correctly.