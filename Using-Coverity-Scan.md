# Setup

1. Download Coverity Scan from https://scan.coverity.com/download
2. Extract and add the Coverity Scan tools to the path
3. Added the FX3 compiler to the Coverity configuration
 - `cov-configure --comptype gcc --compiler /opt/cypress/fx3_sdk/arm-2011.03/bin/arm-none-eabi-gcc`
 - Replace /opt/cypress/fx3_sdk/arm-2011.03/bin/arm-none-eabi-gcc with wherever your FX3 SDK is installed

# Performing a build

Always use the root level CMakeLists.txt to do a build, not the one in host.  If you before a build in host, the fx3 sources will be removed from Coverity.

## Setup common CMake build folder (if needed)
1. cd to root of source tree (e.g. cd ~/bladeRF)
2. `mkdir build`
3. `cd build`
4. `cmake ..`

## Perform the Coverity build
1. cd to root build folder (e.g. cd ~/bladeRF/build)
2. `make clean`
3. `rm -rf cov-int`
4. `cov-build --dir cov-int make`
5. `tar -czvf bladeRF.tgz cov-int/`
6. The tarball can be manually submitted to the website, or you can automatically submit it via 
 - `curl --form project=bladeRF --form token=PUT_YOUR_API_TOKEN_HERE--form email=PUT_YOU_EMAIL@HERE --form file=@bladeRF.tgz --form version=0.4.0 --form description=Description http://scan5.coverity.com/cgi-bin/upload.py`

