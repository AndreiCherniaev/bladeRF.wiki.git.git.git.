# Motivation

The current FX3 firmware loading design has the bladeRF host tools talking directly with the FX3 firmware being replaced.  This means that as the bladeRF host tools and FX3 firmware develop, there is a need to ensure that the bladeRF host tools can load firmware on older FX3 firmwares.  This puts a large testing burden on bladeRF developers to need to maintain backwards compatibility.  However the current maturity of the bladeRF tools and FX3 firmware is low, so that backwards compatibility requirement is onerous.

Already the master bladeRF host tools as of Sept 8th 2013 can no longer flash the factory firmware due to regressions in the bladeRF host tools.  Also issues like https://github.com/Nuand/bladeRF/issues/87 are best fixed will incompatible changes to the USB protocol.

Given the above reasoning, I propose we can the FX3 firmware loading strategy to maximally decouple the different versions of the bladeRF host tools and FX3 firmware from each other.

# Basic idea

The core idea is that the new loading process will only require that the bladeRF host tools match the version of FX3 firmware.  Instead of developers needing to test that the new bladeRF host tools can flash all version, they only need to test that the bladeRF host tools can flash the paired FX3 firmware and that the host tools can return the bladeRF to the FX3 bootloader.

This idea will be accomplished by having the flashing process always get to the FX3 bootloader.  Once there, the bladeRF tools can rely on the FX3 bootloader to load the latest FX3 firmware into RAM.  At that point the bladeRF is loaded with a FX3 firmware that matches the bladeRF tools by definition.

# New Flashing Process

1. The flasher is either given a device id or will search for the presence of either a bladeRF or a FX3 bootloader
1. If the flasher is given/finds a bladeRF device, they get the FX3 on the bladeRF back into its FX3 bootloader.  The flasher will listen for a USB connect of the FX3 bootloader.
1. The flasher will load the new FX3 firmware into RAM and jump to it
1. The flasher will listen for a USB connect of the bladeRF image
1. (Optional) The flasher will load the FX3 firmware into the SPI

# Require code changes

This will require two changes. 

1. A flashing tool will need to created to implement the above procedure.
1. A protocol will need to be defined to get the bladeRF to the FX3 bootloader.  A proposed protocol is given below.

# Protocol to get to FX3 bootloader

For firmware's at version 1.1 or less, either having the user force the bladeRF into FX3 bootloader or erasing the first part of the FX3 firmware is probably the best approaches.  The user will need to manually reset bladeRF once the first part of the FX3 firmware is erased.
For the version 1.2 and 1.3, erasing the first part of the FX3 firmware followed by a reset.
For versions beyond 1.4, I propose we add a JUMP_TO_BOOTLOADER opcode, takes no arguments returns no data, and works in all alturnate selections.

## Flasher protocol

If the flasher detects a bladeRF instead of the FX3 bootloader, it will take the following actions:

1. Attempt to issue the JUMP_TO_BOOTLOADER opcode.  
1. If this has no effect, attempt to erase using the version 1.2/1.3 steps
1. If this has no effect, attempt to erase using the version 1.1 or earlier steps
1. If this has no effect, provide instructions to the user on how to force the FX3 bootloader

# Conclusion

By making an extremely narrow requirement on the FX3 firmware starting beyond 1.3 to have JUMP_TO_BOOTLOADER, the bladeRF host tools no longer need to support older FX3 firmwares beyond the 0.3-1.3 sets explicitly.  This should limit the required testing for FX3 firmware change to just between a paired set of the bladeRF host tools and FX3 firmware.  This will allow FX3 firmware development to make incompatible changes to the rest of the USB protocol without worrying about inability to flash forward or backwards.