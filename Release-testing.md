This page is a collection of thoughts for required testing prior to cutting a release.

Testing required for new FX3 firmware:
* Verify flashing with bladeRF-flash works on Windows, Linux, and OS X on USB 2.0 and USB 3.0
  * Test both RAM-only path and flash paths.  This exercises recover, erase, reset, flash_firmware driver operations.
     * bladeRF-flash -f \<file\>
     * bladeRF-flash -r -l -f \<file\>
  * Test flashing from FX3 firmware versions 1.1 and 1.2.  These are required because they don't have the JUMP_TO_BOOTLOADER vender request.
* Run both USB20CV and USB30CV in compliance testing mode to ensure there are no regressions
* Verify booting the FPGA via USB on Windows, Linux, and OS X on USB 2.0 and USB 3.0
