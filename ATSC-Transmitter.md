Broadcast television in the United States currently uses a set of standards developed by the Advanced Television Systems Committee.  The digital ATSC standards replace the analog National Television System Committee (NTSC) standards for nearly all "major" broadcasters in the US and Canada.

In the typical deployment, ATSC broadcasts use 8VSB modulation in a 6 MHz channel to transmit a 19.39 Mbit/s MPEG Transport Stream, which contains a number of multiplexed video, audio, and data payloads.

# Difficulty Level 1: Transmitting a prepared MPEG Transport Stream

To get to this point, you'll want to have your BladeRF connected to a USB 3.0 port, and be running a recent firmware and hosted FPGA image.  You'll also need to have libbladeRF and gnuradio installed, generally by using the Quick Start instructions elsewhere in this wiki.  I assume you'll be using Linux.

1. Grab a transport stream from somewhere, such as: http://hoopycat.com/bladerf_builds/misc/atsc/advatsc.ts (246 MB)
2. Grab atsc-blade.py: https://gist.github.com/rtucker/8641650
3. Make sure a "reasonable" UHF antenna is attached to your BladeRF's transmit port, and that physical channels 14 (470 to 476 MHz) and 15 (476 to 482 MHz) are unused in your area -- if not, adjust center_freq to land on some other channel.
4. Turn on your DTV receiver and set it to broadcast channel 14 (or whatever you chose).
5. Roll that beautiful bean footage: ```python atsc-blade.py advatsc.ts```

You should see this in your terminal window:

```
Transmitting pre-encoded content: hdenc/advatsc.ts
Using Volk machine: avx_64_mmx_orc
gr-osmosdr v0.1.0-55-g80c4af4f (0.1.1git) gnuradio 3.7.1
built-in sink types: bladerf 
[bladeRF source] Using nuand LLC bladeRF #0 SN [redacted] FW v1.6.1 FPGA v0.0.2
```

You should see this on your spectrum analyzer:

![Photograph of spectrum analyzer showing a 6 MHz wide 8-VSB signal centered on 483 MHz.](http://hoopycat.com/bladerf_builds/misc/atsc/atsc-1.jpg)

And you should see this on your television!

![Photograph of television program with on-screen display showing a HD signal on digital channel 14-1.](http://hoopycat.com/bladerf_builds/misc/atsc/atsc-2.jpg)