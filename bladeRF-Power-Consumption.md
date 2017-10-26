# Overview #

Power consumption of the bladeRF will change depending on several factors. The largest contributors are: FPGA size, FPGA utilization, expansion boards, ambient temperature, sample rate, and the LNA gain setting. There will also be individual variations between bladeRF boards due to normal device tolerances and differences between manufacturing lots; however, these should be fairly minimal by comparison. This page catalogs the power consumption of some typical bladeRF configurations.

## Background and Test Setup ##

Although a power supply may output 5&nbsp;V nominally, not all supplies (motherboards, AC power adapters, etc.) are created equally. There are variations in the exact output voltage and current sourcing capabilities; and it is common for power supplies to experience voltage droop as the load current increases.

The bladeRF under test was configured to accept power over the DC barrel jack. The DC barrel jack was connected to a Rigol DP832 bench power supply set to 5&nbsp;V. A Fluke 87&#8209;V multimeter was used to measure voltage at J49 of the bladeRF to account for any voltage drop across the wires from the power supply. Power was then computed by multiplying the current displayed on the bench supply with the voltage displayed on the DMM. The power supply and the DMM both display 3 fractional digits.

All measurements were taken at a room temperature of 72&deg;F (22.4&deg;C) in still air. No enclosure was used, and no heat sinks or fans of any kind were used. The bladeRF was allowed to "warm up" for approximately 10&nbsp;minutes prior to recording any power readings. Stock "hosted" FPGA images were used. Transmit testing was performed with a continuous waveform at the center frequency.

A final test was performed to determine the amount of acceptable voltage droop before the bladeRF lost its connection to the host machine. This test was repeated in the opposite direction to determine the voltage point at which the bladeRF powers up and is detected by the host.

## TL;DR ##

* Expect a **bladeRF alone** to consume up to **4&nbsp;W** at room temperature.
* Expect a **bladeRF + XB-200** to consume up to **4.5&nbsp;W** at room temperature.
* Expect a **bladeRF + XB-300** to consume up to **10&nbsp;W** at room temperature.
* Minimum supply voltage before disconnect: **2.950&nbsp;V**
* Minimum supply voltage to power up: **3.500&nbsp;V**

A standard **USB&nbsp;2.0** port can deliver up to **2.5&nbsp;W** of power. A standard **USB&nbsp;3.0** port can deliver up to **4.5&nbsp;W** of power. Deviations from these standards exist, such as high-power USB ports. The bladeRF will work on many USB 2.0 ports (with limitations). *Your mileage may vary!* Consider using an external AC adapter if your power requirements are expected to exceed the USB host bus power specifications.

# Detailed Measurements #

## UUT information: ##

| UUT         | Type | bladeRF-cli        | libbladeRF         | Firmware | FPGA  |
| ----------- | :--: | ------------------ | ------------------ | -------- | ----- |
| fce6..062c  | x40  | 1.5.0-git-6116d090 | 1.8.0-git-6116d090 | 2.0.0    | 0.6.0 |
| fd4b..7d7f  | x115 | 1.5.0-git-6116d090 | 1.8.0-git-6116d090 | 2.0.0    | 0.6.0 |

More UUTs may be added at some point in the future, time and resources permitting. Please consider contacting us if you have data you would like to contribute.

## General / RF Idle ##

| State                                             | x40 Power (W) | x115 Power (W) |
| ------------------------------------------------- | :-----------: | :------------: |
| Unconfigured                                      | 0.717         | 0.717          |
| Unconfigured, with XB-200                         | 0.717         | 0.717          |
| Unconfigured, with XB-300                         | 0.731         | 0.731          |
|                                                   |               |                |
| Configured, FX3 GPIF idle, RF idle                | 2.136         | 2.176          |
| Configured, FX3 GPIF idle, RF idle, with XB-200   | 2.136         | 2.177          |
| Configured, FX3 GPIF idle, RF idle, with XB-300   | 2.161         | 2.194          |
|                                                   |               |                |
| Configured, FX3 GPIF active, RF idle              | 2.204         | 2.250          |
| Configured, FX3 GPIF active, RF idle, with XB-200 | 2.204         | 2.251          |
| Configured, FX3 GPIF active, RF idle, with XB-300 | 2.223         | 2.276          |

## FX3 GPIF, FPGA, and RF Active ##

### bladeRF x40 ###

| RX | TX | RX / TX Freq (MHz) | IBW (MHz) / SR (MSPS) | RX Gains (dB) <br/> LNA / VGA1 / VGA2 | TX Gains (dB) <br/> VGA1 / VGA2 | Power (W) |
| :-: | :-: | :--------: | :----------------------------------: | :-----------------------------------: | :-----------------------------: | :-------: |
| ON  | OFF | 3800 / 3798  | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.441 |
| ON  | OFF | 3800 / 3798  | 2.5 / 5 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.488 |
|     |     |              |         |                   |               |       |
| ON  | OFF | 3800 / 3798  | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.580 |
| ON  | OFF | 3800 / 3798  | 28 / 40 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.662 |
|     |     |              |         |                   |               |       |
| ON  | OFF | 302 / 300    | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.484 |
| ON  | OFF | 302 / 300    | 2.5 / 5 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.540 |
|     |     |              |         |                   |               |       |
| ON  | OFF | 302 / 300    | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.624 |
| ON  | OFF | 302 / 300    | 28 / 40 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.739 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 3800 / 3798  | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.628 |
| ON  | ON  | 3800 / 3798  | 2.5 / 5 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.214 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 3800 / 3798  | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.782 |
| ON  | ON  | 3800 / 3798  | 28 / 40 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.362 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 302 / 300    | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.686 |
| ON  | ON  | 302 / 300    | 2.5 / 5 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.386 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 302 / 300    | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.831 |
| ON  | ON  | 302 / 300    | 28 / 40 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.529 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 302 / 300    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **3.644** |

### bladeRF x40 & XB-200 ###

The XB-200 filter paths were set to "auto_3db" to automatically select the band based on center frequency.

| RX | TX | RX / TX Freq (MHz) | IBW (MHz) / SR (MSPS) | RX Gains (dB) <br/> LNA / VGA1 / VGA2 | TX Gains (dB) <br/> VGA1 / VGA2 | Power (W) |
| :-: | :-: | :--------: | :----------------------------------: | :-----------------------------------: | :-----------------------------: | :-------: |
| OFF | OFF | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.269 |
| ON  | OFF | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.589 |
| OFF | ON  | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.081 |
| ON  | ON  | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **4.410** |
| ON  | ON  | 54 / 50      | 2.5 / 5 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.228 |
| ON  | ON  | 54 / 50      | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 3.570 |
|     |     |              |         |                   |               |       |
| OFF | OFF | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.255 |
| ON  | OFF | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.575 |
| OFF | ON  | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.100 |
| ON  | ON  | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **4.420** |
| ON  | ON  | 225 / 222    | 2.5 / 5 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.246 |
| ON  | ON  | 225 / 222    | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 3.555 |

### bladeRF x40 & XB-300 ###

During these tests, the state of the LNA and PA on the XB-300 matched the state of the RX and TX activity, respectively. In other words, when receiving, the LNA was powered up; and when transmitting, the PA was powered up. Conversely, when not receiving and/or transmitting, the LNA and/or PA was powered down.

| RX | TX | RX / TX Freq (MHz) | IBW (MHz) / SR (MSPS) | RX Gains (dB) <br/> LNA / VGA1 / VGA2 | TX Gains (dB) <br/> VGA1 / VGA2 | Power (W) |
| :-: | :-: | :--------: | :----------------------------------: | :-----------------------------------: | :-----------------------------: | :-------: |
| OFF | OFF | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 2.314 |
| ON  | OFF | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.133 |
| OFF | ON  | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 8.584 |
| ON  | ON  | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **9.013** |

### bladeRF x115 ###

| RX | TX | RX / TX Freq (MHz) | IBW (MHz) / SR (MSPS) | RX Gains (dB) <br/> LNA / VGA1 / VGA2 | TX Gains (dB) <br/> VGA1 / VGA2 | Power (W) |
| :-: | :-: | :--------: | :----------------------------------: | :-----------------------------------: | :-----------------------------: | :-------: |
| ON  | OFF | 3800 / 3798  | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.506 |
| ON  | OFF | 3800 / 3798  | 2.5 / 5 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.568 |
|     |     |              |         |                   |               |       |
| ON  | OFF | 3800 / 3798  | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.657 |
| ON  | OFF | 3800 / 3798  | 28 / 40 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.759 |
|     |     |              |         |                   |               |       |
| ON  | OFF | 302 / 300    | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.560 |
| ON  | OFF | 302 / 300    | 2.5 / 5 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.613 |
|     |     |              |         |                   |               |       |
| ON  | OFF | 302 / 300    | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.706 |
| ON  | OFF | 302 / 300    | 28 / 40 | 6 / 30 / 30 (max) | -35 / 0 (min) | 2.814 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 3800 / 3798  | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.708 |
| ON  | ON  | 3800 / 3798  | 2.5 / 5 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.320 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 3800 / 3798  | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.864 |
| ON  | ON  | 3800 / 3798  | 28 / 40 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.474 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 302 / 300    | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.765 |
| ON  | ON  | 302 / 300    | 2.5 / 5 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.487 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 302 / 300    | 28 / 40 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 2.923 |
| ON  | ON  | 302 / 300    | 28 / 40 | 0 / 5 / 0 (min)   | -4 / 25 (max) | 3.638 |
|     |     |              |         |                   |               |       |
| ON  | ON  | 302 / 300    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **3.782** |

### bladeRF x115 & XB-200 ###

The XB-200 filter paths were set to "auto_3db" to automatically select the band based on center frequency.

| RX | TX | RX / TX Freq (MHz) | IBW (MHz) / SR (MSPS) | RX Gains (dB) <br/> LNA / VGA1 / VGA2 | TX Gains (dB) <br/> VGA1 / VGA2 | Power (W) |
| :-: | :-: | :--------: | :----------------------------------: | :-----------------------------------: | :-----------------------------: | :-------: |
| OFF | OFF | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.349 |
| ON  | OFF | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.672 |
| OFF | ON  | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.190 |
| ON  | ON  | 54 / 50      | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **4.500** |
| ON  | ON  | 54 / 50      | 2.5 / 5 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.331 |
| ON  | ON  | 54 / 50      | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 3.655 |
|     |     |              |         |                   |               |       |
| OFF | OFF | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.347 |
| ON  | OFF | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.669 |
| OFF | ON  | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.234 |
| ON  | ON  | 225 / 222    | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **4.557** |
| ON  | ON  | 225 / 222    | 2.5 / 5 | 6 / 30 / 30 (max) | -4 / 25 (max) | 4.365 |
| ON  | ON  | 225 / 222    | 2.5 / 5 | 0 / 5 / 0 (min)   | -35 / 0 (min) | 3.641 |

### bladeRF x115 & XB-300 ###

During these tests, the state of the LNA and PA on the XB-300 matched the state of the RX and TX activity, respectively. In other words, when receiving, the LNA was powered up; and when transmitting, the PA was powered up. Conversely, when not receiving and/or transmitting, the LNA and/or PA was powered down.

| RX | TX | RX / TX Freq (MHz) | IBW (MHz) / SR (MSPS) | RX Gains (dB) <br/> LNA / VGA1 / VGA2 | TX Gains (dB) <br/> VGA1 / VGA2 | Power (W) |
| :-: | :-: | :--------: | :----------------------------------: | :-----------------------------------: | :-----------------------------: | :-------: |
| OFF | OFF | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 2.424 |
| ON  | OFF | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 3.227 |
| OFF | ON  | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | 8.614 |
| ON  | ON  | 2402 / 2400  | 28 / 40 | 6 / 30 / 30 (max) | -4 / 25 (max) | **9.204** |

## Minimum Supply Voltage ##

The DC barrel voltage was continuously reduced until the host lost its connection with the bladeRF device. This minimum voltage was determined to be **2.950&nbsp;V**, matching the datasheet of the LM2014x regulators.

This test was repeated in the opposite direction. The DC barrel voltage was continuously increased until the host was able to detect the bladeRF device. This minimum power on voltage was determined to be **3.500&nbsp;V**.

These experiments are provided for informational purposes only. Nuand does not recommend operating at these voltage extremes. And as always, *your mileage may vary!*
