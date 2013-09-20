There are several deficiencies in the FX3 firmware that should be resolved.  At this point it probably no longer makes sense to make individual bugs for each, so instead I am list them here.  I will begin burning down the list when I have time.  If other people want to help, drop me a line and we can divide and conquer.

 - [FX3 firmware does not provide descriptive error codes when a failure occurs](https://github.com/Nuand/bladeRF/issues/87)
 - [FX3 firmware does not always check error codes from Cy API](https://github.com/Nuand/bladeRF/issues/88)
 - UART errors are not detected (framing/overflow)
 - SPI errors are not detected (underflow/overflow)
 - Once UART errors are detected, no way to report counters.  Need to develop diagnostic data interface.
 - It would be really nice to have logging support in the FX3 firmware.
 - Fatal errors in the FX3 firmware result in lockup.  Better to some how report to host problem occured.
 - bladeRF.c is large in terms of line count.  Refactor into modules.
 - Implement suspend/resume (low priority)
 - Add git rev to fx3 firmware build

Open questions:
 - Order of shutting down EPs and DMAs.  Should halt EP then DMA, or DMA then EP.  Or either works?
 - DMA for RF data is byte oriented.  Should be symbol oriented?
 - Is bladeRFConfigUtoPDmaCallback truly required?
 - Unclear if endpoint halt feature is implemented correctly.