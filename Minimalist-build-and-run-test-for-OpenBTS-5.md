## What is this?
These are notes jotted down from IRC logs from `rwr` helping `mambrus` setting up a minimalist OpenBTS 5 one late night 2014-10-02.

No guarantees it will work, but hopefully rather serve as a base for future documentation. (Let me know if it does or doesn't and we can skip this caution-note.)

## Condensed **do-list**
Follow the links where such exist. The text referred to are usually not large and I've tried to be specific. Some of the links are just for future referral, some of them contain code snippets that should had gone in here instead. As I said, these are just jotted notes atm ;-)

* RangeNetworks [gits](https://github.com/RangeNetworks/) <-- are here
* Prerequisits. 
   * Install your distos packages. No need to build from source.
     * libgsm1-dev
     * asterisk-dev
     * asterisk-config
   * May need build...
     * Very recent libusb. If built from source for example for Ubuntu 12.04:
       * copy that over /usr/lib/x86_64-linux-gnu/libusb.so (consider saving the old one)
* Prepare a directory for OpenBTS_5 like [this](http://pastebin.com/HeWUddAE)
* In the same root directory clone [YateBTS](http://wiki.yatebts.com/index.php/SVN)
  * *Note:* you don't need the Yate tool
  * Remove loading of fpga in YateBTS
    * `vim ./yatebts/mbts/TransceiverRAD1/bladeRFDevice.cpp +108` and onward to line 129. `#ifdef NEVER` it...
      * Make sure you have a recent fpga and matching firmwatre in bladeRF (install fpga permanently: `bladeRF-cli  -L path/file`, Make sure not to interrupt flashing) 
  * Build only 2 directories (order matters):
     * cd into `Peering`, run make
     * cd into `TransceiverRAD1` and make
  * Copy produced transceiver binary from YateBTS to OpenBTS:
```bash
cd ..
cp ./yatebts/mbts/TransceiverRAD1/transceiver-bladerf openbts/apps/
cd openbts/apps/
ln -sf transceiver-bladerf transceiver
```  
* enter openbts and run `autogen.sh`
* Run `./configure --with-uhd`
* Configure mysql DB according to [this](https://wush.net/trac/rangepublic/wiki/BuildInstallRun#ConfiguringOpenBTS) but patch `apps/OpenBTS.example.sql` first:
  * GSM.Radio.RxGain from 47 to **5** in 
  * GSM.Radio.PowerManager.MaxAttenDB to **35**
  * GSM.Radio.PowerManager.MinAttenDB to **35**
* Continue from the DB configuration and onwards
   * OpenBTS should be running and output similar to [this](http://pastebin.com/GPHu3DBG)
* Phone search for networks will show [this](http://picpaste.com/2014-10-03_16.57.40-gIoaKgVA.jpg)

* Did you read the OpenBTS document carefully? Then you know how to make your BTS accept your phone. Which could look somewhat like [this](http://picpaste.com/2014-10-03_15.04.53-qDwsRkrO.png)
 