# Requests
* Add jumper to force FX3 into built-in bootloader
* Provide access to SPI flash through a header directly.  The MOSI and MISO are already brought out via the UART header.  Adding the CS and SCLK would allow direct programming of the FX3 SPI flash.