## What is this?
These are notes jotted down from IRC logs from `rwr` helping `mambrus` setting up a minimalistic [OpenBTS **pre-release** version 5](http://openbts.org/) using [YateBTS](http://wiki.yatebts.com/index.php/Main_Page) transceiver one late night `2014-10-02`.

No guarantees it will work, but hopefully rather serve as a base for future documentation. (Let me know if it does or doesn't and we can skip this caution-note and/or refine this into a proper how-to.)

## Prerequisites
Copy & paste the following for a Debian-based system:
```bash
sudo apt-get install $(
    wget -qO - https://raw.githubusercontent.com/RangeNetworks/dev/master/build.sh | \
    grep installIfMissing | \
    grep -v "{" | \
    cut -f2 -d" ")
```

Hopefully all the packages are in your distros repo and are named the same. If not, modify the lines above and add `grep -v <package>` for the packages that fail, then find another way to install them. Building from source is always an alternative...

### Alternative ways of installing cumbersome packages.

This section describe alternative ways of installing some of the packages above known to be missing for some Debian-stable distros.

#### libzmq3 & libzmq3-dev
* For Ubuntu:
```bash
$ sudo add-apt-repository ppa:chris-lea/zeromq
$ sudo apt-get update
$ sudo apt-get install libzmq3-dbg libzmq3-dev 
```
For Debian 7.2(Wheezy) you can use `libzmq3` & `libortp8` & Debian Sid you can try `libzmq3-dev` & `libortp9` (**Note:** Confirmation needed)

#### uhd
Build and install [gnuradio from source](http://gnuradio.org/redmine/projects/gnuradio/wiki/InstallingGRFromSource#Using-the-build-gnuradio-script)

## Condensed **do-list**
Follow the links where such exist. The text referred to are usually not large and I've tried to be specific. Some of the links are just for future referral, some of them contain code snippets that should had gone in here instead. As I said, these are just jotted notes atm ;-)

* RangeNetworks [gits](https://github.com/RangeNetworks/) <-- are here
* Prerequisites. 
   * Install your distributions packages (no need to build from source).
     * libgsm1-dev
     * asterisk-dev
     * asterisk-config
   * May need build...
     * Very recent `libusb`. If built from source (for example for Ubuntu 12.04):
       * copy that over /usr/lib/x86_64-linux-gnu/libusb.so (consider saving the old one)
* Prepare a directory for OpenBTS_5 as follows:
```bash
#!/bin/bash
 
git clone https://github.com/RangeNetworks/openbts.git
git clone https://github.com/RangeNetworks/smqueue.git
git clone https://github.com/RangeNetworks/subscriberRegistry.git

#From here and downwards you can copy&paste (that's why the ';' are for)
for D in *; do (
    echo $D;
    echo "=======";
    cd $D;
    git clone https://github.com/RangeNetworks/CommonLibs.git;
    git clone https://github.com/RangeNetworks/NodeManager.git);
done;
git clone https://github.com/RangeNetworks/libcoredumper.git;
git clone https://github.com/RangeNetworks/liba53.git
```

* Build libraries libcoredumper & liba53
```bash
cd libcoredumper;
./build.sh && \
   sudo dpkg -i *.deb;
cd ..
```

```bash
cd liba53;
make && \
   sudo make install;
cd ..;
```

* In the same root directory clone YateBTS
  * `svn checkout --revision 503 http://voip.null.ro/svn/yatebts/trunk yatebts`
  * *Note:* **You need YateBTS SVN revision 503 or older!** In later SVN revisions the RAD1 based transceiver that can be used with OpenBTS or OsmoBTS was removed.
  * *Note:* you don't need the Yate tool
  * Remove loading of fpga in YateBTS
    * `vim ./yatebts/mbts/TransceiverRAD1/bladeRFDevice.cpp +108` and onward to line 129. `#ifdef NEVER` it...
      * Make sure you have a recent fpga and matching firmware in bladeRF (install fpga permanently: `bladeRF-cli  -L path/file`, Make sure not to interrupt flashing)
  * Go to YateBTS and run `autogen.sh` to get a configure script.
  * Run `configure`
    * Which will fail with an error complaining about yate. Find the error in `configure` and fix it (patch it to ignore the error in any preferable way). Then rerun `configure`. 
  * You need to build only 2 directories from `yatebts` to get the transceiver (order matters):
*Note:* I've noticed that `yatebts` does not support out of source build.
     * cd into `Peering`, run make
     * cd into `TransceiverRAD1` and make
  * Copy produced transceiver binary from YateBTS to OpenBTS:
```bash
cd ..
cp ./yatebts/mbts/TransceiverRAD1/transceiver-bladerf openbts/apps/
cd openbts/apps/
ln -sf transceiver-bladerf transceiver
```

* build OpenBTS.
This is done very traditionally but spelled out here because of the configure flag.
  * enter openbts and run `autogen.sh`
  * Run `./configure --with-uhd`
  * make
* Configure sqlite3 DB according to [this](https://wush.net/trac/rangepublic/wiki/BuildInstallRun#ConfiguringOpenBTS) but patch `apps/OpenBTS.example.sql` first:
  * `GSM.Radio.RxGain` from 47 to **5** in 
  * `GSM.Radio.PowerManager.MaxAttenDB` to **35**
  * `GSM.Radio.PowerManager.MinAttenDB` to **35**
* Continue from the DB configuration part and onward
   * When done OpenBTS should be running and output interactive console similar to [this](http://pastebin.com/GPHu3DBG)
* Phone search for networks should show something similar to:

<a href="http://picpaste.com/2014-10-03_16.57.40-gIoaKgVA.jpg"><img src="http://picpaste.com/extpics/2014-10-03_16.57.40-gIoaKgVA.jpg" alt="PicPaste: 2014-10-03_16.57.40-gIoaKgVA.jpg" /></a>

* Did you read the OpenBTS document referred to [above](https://wush.net/trac/rangepublic/wiki/BuildInstallRun#RunningOpenBTS) carefully? Then you know how to make your BTS accept your phone (look for `Control.LUR.OpenRegistration`). End-result could then look something like this:

<a href="http://picpaste.com/2014-10-03_15.04.53-qDwsRkrO.png"><img src="http://picpaste.com/extpics/2014-10-03_15.04.53-qDwsRkrO.png" alt="PicPaste: 2014-10-03_15.04.53-qDwsRkrO.png" /></a>

*Please do take care about following local regulations.*

## History of the bladeRF compatible GSM transceiver
* In march 2014 @robertghilduta (Robert Ghilduta, Nuand LLC) released the initial transceiver code based on Tranceiver52M
  * https://github.com/Nuand/YateBTS/commit/d0cb6b2863abb5d0ffadd1cd4770b3df52365317
* In april 2014 @robertghilduta switched to TransceiverRAD1 since there were some problems with Transceiver52M
  * https://github.com/Nuand/YateBTS/commit/36cb7dd06dcf33647ba3307781ff9cf1a271872a
* In November 2014 l-fy (Diana Cionoiu, Null.ro) gave a notice that the Yate-team will deprecate the RAD1 based transceiver soon
  * `2014-11-07 18:06:22" < l-fy> and we will deprecate transciever rad1`
* Revision 1 - 2014-11-06 15:33:57 GMT - initial version of the **new** yate-transceiver (**not compatible with bladeRF!**)
  * http://yate.null.ro/websvn/listing.php?repname=bladeRF  (repository now depreciated)
* On 30.01.2015 The Yate team officially announced that there will be a **new** transceiver written from scratch
* On 24.06.2016 The **new** yate transceiver made it into their yatebts-repo and at the same time the they deleted the old transceivers (Transceiver52M and TransceiverRAD1)
  * delete commit http://yate.null.ro/websvn/revision.php?repname=yatebts&path=%2F&rev=504
  * The **new transceiver only works with YateBTS** since they made the transceiver interface (for some unknown reason) incompatible to OpenBTS and OsmoBTS from the osmocom project. :/






## Updates

* 2014-11-04 fbe0b6a8 breaks YateBTS transceiver

Prior to [fbe0b6a8](https://github.com/Nuand/bladeRF/commit/fbe0b6a81e251517fa6265211809b8e66b781d50#commitcomment-8413061), bladerf_sync_config()did not touch the timestamp bit as there was no support for dealing with timestamps.

There's a patch proposal to Yate at the following link: [Yate_transceiver_revert_init_order.patch](http://pastebin.com/G1Y32Z9a). Patch is pending accept and until further notice you have to patch in YateBTS source manually (`patch -p0 < the.patch`) to be able to use the YateBTS transceiver.


* 2015-02-10 No need to patch Yate tranciever anymore

The fine folks of YateBTS has fixed the transceiver. No need to patch anymore. 

It was done in two steps.

1) YateBTS incorporated the patch in their source
2) Tranciever has been re-written from scratch.

Not sure about what production status currently is and which of the above points apply. Main thing is however, you don't need to patch (you still need to build though).