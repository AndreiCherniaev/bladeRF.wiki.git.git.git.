

| Argument | Notes |
| -------- | ----- |
| bladerf[=instance\|serial] | Selects the specified bladeRF device by a 0-indexed "device instance" count or by the device's serial number. 3 or more characters from the serial number are required. If 'instance' or 'serial' are not specified, the first available device is used. |
| fpga=\<'/path/to/the/bitstream.rbf'\> | Load the FPGA bitstream from the specified file. This is required only once after powering the bladeRF on. If the FPGA is already loaded, this argument is ignored, unless 'fpga-reload=1' is specified. |
| fpga-reload=1 | Force the FPGA to be reloaded. Requires fpga=<bitrstream> to be provided to have any effect. |
| buffers=\<count\> | Number of sample buffers to use. Increasing this value may alleviate transient timeouts, with the trade-off of added latency. This must be greater than the 'transfers' parameter. Default=32 |
| buflen=\<count\> | Length of a sample buffer, in samples (not bytes). This must be a multiple of 1024. Default=4096 |
| transfers=\<count\> | Number of in-flight sample buffer transfers. Defaults to one half of the 'buffers' count. |
| stream_timeout_ms=\<timeout\> | Specifies the timeout for the underlying sample stream. Default=3000. |
| loopback=\<mode\> | Configure the device for the specified loopback mode (disabled, baseband, or RF). See the libbladeRF documentation for descriptions of these available options: none, bb_txlpf_rxvga2, bb_txlpf_rxlpf, bb_txvga1_rxvga2, bb_txvga1_rxlpf, rf_lna1, rf_lna2, rf_lna3. The default mode is 'none'. |
| verbosity=\<level\> | Controls the verbosity of output written to stderr from libbladeRF. The available options, from least to most verbose are: silent, critical, error, warning, info, debug, verbose. The default level is determined by libbladeRF. |
| xb200[=filter] | Automatic filter selection will be enabled if no value is given to the xb200 parameter. Otherwise, a specific filter may be selected per the list presented below. |
| smb=\<frequency\> | Enable frequency output on SMB connector (named CLK) |
| tamer=[internal,external,external_1pps] | Set one of the clock input modes |

The following values are valid for the xb200 parameter:

| Value     | Description |
|-----------|-------------|
| "custom"  | custom band |
| "50M"     |  50MHz band |
| "144M"    | 144MHz band |
| "222M"    | 222MHz band |
| "auto3db" | Select filterbank based on -3dB filter points |
| "auto"    | Select filterbank based on -1dB filter points (default) |

gr-osmosdr <-> bladeRF x40/x115 gain mappings

Sink:

| Setting | Component |
|---------|-----------|
| BB Gain | TX VGA1 [-35, -4] |
| IF Gain | N/A |
| RF Gain | TX VGA2 [0, 25] |

Source:

| Setting | Component |
|---------|-----------|
| RF Gain | LNA Gain {0, 3, 6} |
| IF Gain | N/A |
| BB Gain | RX VGA1 + RX VGA2 [5, 60] |