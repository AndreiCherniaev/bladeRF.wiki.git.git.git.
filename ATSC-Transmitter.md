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
Transmitting pre-encoded content: advatsc.ts
Using Volk machine: avx_64_mmx_orc
gr-osmosdr v0.1.0-55-g80c4af4f (0.1.1git) gnuradio 3.7.1
built-in sink types: bladerf 
[bladeRF source] Using nuand LLC bladeRF #0 SN [redacted] FW v1.6.1 FPGA v0.0.2
```

You should see this on your spectrum analyzer:

![Photograph of spectrum analyzer showing a 6 MHz wide 8-VSB signal centered on 483 MHz.](http://hoopycat.com/bladerf_builds/misc/atsc/atsc-1.jpg)

And you should see this on your television!

![Photograph of television program with on-screen display showing a HD signal on digital channel 14-1.](http://hoopycat.com/bladerf_builds/misc/atsc/atsc-2.jpg)

Congratulations!  You are now transmitting an ATSC signal!  If it doesn't work straight away, check that you aren't getting a lot of overflow/underflow errors, and that LEDs 1 and 3 are *solid* on your BladeRF.  Also, make sure your TV is set to tune to broadcast channel 14, not cable channel 14.

Press ctrl-C when finished.

# Difficulty Level 2: Generating an MPEG Transport Stream for broadcast

What, you don't like Adventure Time?

If you have a video file that you want to try transmitting, install avconv (aka ffmpeg) and take a look at atsc-ts-converter.bash: https://gist.github.com/rtucker/8641882

This script specifies a video file (tbm_tdf_2.mp4) and saves it to a transport stream file (tbm_tdf_2.ts).  It also puts a text overlay on it.  Feel free to change TEXT, VIDEOFILE, and OUTFILE to suit your needs.

Note that the video just has to be in a format avconv can understand -- it will do the transcoding into something your TV should understand.

Be warned that MPEG Transport Streams are *big*:

```
-rw-r--r-- 1 rtucker rtucker  23839649 Dec 29 20:23 tbm_tdf_2.mp4
-rw-r--r-- 1 rtucker rtucker 792414924 Dec 29 20:54 tbm_tdf_2.ts
```

But once you have it generated, you can play it back, and it should work:

```
$ ./atsc-blade.py ~/Videos/tbm_atsc_test/tbm_tdf_2.ts 
Transmitting pre-encoded content: /home/rtucker/Videos/tbm_atsc_test/tbm_tdf_2.ts
Using Volk machine: avx_64_mmx_orc
gr-osmosdr v0.1.0-55-g80c4af4f (0.1.1git) gnuradio 3.7.1
built-in sink types: bladerf 
[bladeRF source] Using nuand LLC bladeRF #0 SN [redacted] FW v1.6.1 FPGA v0.0.2
```

![Photograph of television program with female singer.](http://hoopycat.com/bladerf_builds/misc/atsc/atsc-3.jpg)

(Yes, my television is an old GoldStar CRT with a cheap government-cheese DTV converter box.  Stop laughing at me.  I don't really watch television...)

# Difficulty Level 3: "Live" transcoding of content

(under construction)

# Difficulty Level 4: Live capture and encoding of content

(under construction)
