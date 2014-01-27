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

For this, I'm going to turn one of my bicycle commutes into a music video.  I mounted a crappy camera (we'll call it a GoCheap) to my bicycle and got a MJPEG video with a lot of gearshifting noise and heavy breathing on the audio track, so we'll replace that with some pleasant music.

1. Grab atsc-ts-streamer.bash from my gist: https://gist.github.com/rtucker/8642061
2. Update VIDEOFILE and AUDIOFILE to point at some video and audio
3. In one terminal window, run ```python atsc-blade.py``` without any arguments
4. In another terminal window, run ```./atsc-ts-streamer.bash```:

```
avconv version 0.8.9-6:0.8.9-0ubuntu0.13.10.1, Copyright (c) 2000-2013 the Libav developers
  built on Nov  9 2013 19:09:46 with gcc 4.8.1
Input #0, avi, from '/home/rtucker/Videos/Bike Cam/2012-09-09-002-afternoon.AVI':
  Duration: 00:16:20.15, start: 0.000000, bitrate: 9442 kb/s
    Stream #0.0: Video: mjpeg, yuvj422p, 640x480, 30 tbr, 30 tbn, 30 tbc
    Stream #0.1: Audio: pcm_mulaw, 8000 Hz, 1 channels, s16, 64 kb/s
[mp3 @ 0x19aa7c0] max_analyze_duration reached
Input #1, mp3, from '/home/rtucker/Music/Track N Field/Marathon/04 - Track N Field - Iso Maha.mp3':
  Metadata:
    album           : Marathon
    genre           : Broken beat/Nu Jazz/Nu Soul
    copyright       : 2008 Nine2Five
    TSRC            : DEY470803172
    title           : Iso Maha
    artist          : TRACK N FIELD
    album_artist    : TRACK N FIELD
    publisher       : para-dise.biz
    track           : 4
    date            : 2008-08-01
  Duration: 00:06:45.02, start: 0.000000, bitrate: 321 kb/s
    Stream #1.0: Audio: mp3, 44100 Hz, stereo, s16, 320 kb/s
Incompatible pixel format 'yuvj422p' for codec 'mpeg2video', auto-selecting format 'yuv420p'
[buffer @ 0x196b740] w:640 h:480 pixfmt:yuvj422p
[scale @ 0x19fc020] w:640 h:480 fmt:yuvj422p -> w:1280 h:720 fmt:yuv420p flags:0x4
[mpegts @ 0x19aee00] muxrate 19393000, pcr every 257 pkts, sdt every 6447, pat/pmt every 1289 pkts
Output #0, mpegts, to 'udp://127.0.0.1:1234?pkt_size=188&buffer_size=65535':
  Metadata:
    service_name    : udp://127.0.0.1:1234?pkt_size=188&buffer_size=65535
    service_provider: BladeRF ATSC Demo
    encoder         : Lavf53.21.1
    Stream #0.0: Video: mpeg2video, yuv420p, 1280x720, q=2-31, 2000 kb/s, 90k tbn, 30 tbc
    Stream #0.1: Audio: ac3, 48000 Hz, stereo, s16, 384 kb/s
Stream mapping:
  Stream #0:0 -> #0:0 (mjpeg -> mpeg2video)
  Stream #1:0 -> #0:1 (mp3 -> ac3_fixed)
Press ctrl-c to stop encoding
frame= 2172 fps= 30 q=31.0 Lsize=  171281kB time=72.37 bitrate=19389.3kbits/s 
```

And boom!  You should see it on your television, with your chosen music on the audio channel.

![Photograph of television displaying a video from a bicycle traversing a city roadway.](http://hoopycat.com/bladerf_builds/misc/atsc/atsc-4.jpg)

You might notice the signal breaks up more than you'd like.  The transcoding and multiplexing are being done in real time and that is somewhat stressful.  Ways to improve this are welcome.

# Difficulty Level 4: Live capture and encoding of content

So all that was boring.  Why don't we try something more exciting?  My laptop has a webcam.

Copy the ```atsc-ts-streamer.bash``` script to a machine on the same ethernet segment as the machine running ```atsc-blade.py``` (or maybe the same machine, if you have a webcam!) and set the following:

```
VIDEOFILE="-f video4linux2 -r 30 -s vga -i /dev/video0"
AUDIOFILE="-i http://audioplayer.wunderground.com:80/dampier/rochester.mp3"   # or some other suitable streaming audio content
OUTFILE="udp://192.168.1.194:1234?pkt_size=188&buffer_size=65535"   # replace 192.168.1.194 as required
```

Start up ```atsc-blade.py``` with no arguments, and then, on your webcam machine, start ```atsc-ts-streamer.bash```.  If all goes well, holy crow!


