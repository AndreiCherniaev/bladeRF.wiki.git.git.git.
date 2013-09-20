# Requests
* Add jumper to force FX3 into built-in bootloader (workaround: http://nuand.com/forums/viewtopic.php?f=6&t=3072)
* Provide access to SPI flash through a header directly.  The MOSI and MISO are already brought out via the UART header.  Adding the CS and SCLK would allow direct programming of the FX3 SPI flash.
* FX3 debug LED is not very bright
* Align GPIF0-7 with DATA0-7.  Currently parallel programming of the FPGA via the FX3 required a weird mask translation.