# Firmware Update

### Intro

This is a guide to compiling and updating the firmware on the AscTec FireFly.
We need to flash the firmware so we can get the correct events from the high level processor.
If this is not done then the normal error will be a constant `rx timeout` and `waiting for acknowledged packet timed out` error.
This can be caused by using the wrong port (`/dev/ttyS2` for mastermind and `/dev/ttyUSB0` for atom) or because the firmware on the high level processor is incompatible.

### Requirements

 1. Install OpenOCD:
    - `sudo apt-get install openocd`
 2. For 64 bit Ubuntu also install:
    - `sudo apt-get install libncurses5-dev:i386`
    - `sudo apt-get install libx11-dev:i386`
    - `sudo apt-get install zlib1g:i386`
 3. Install an arm-elf-gcc compiler.
    - Available [here](http://wiki.asctec.de/display/AR/SDK+Downloads)
 4. Download the most recent SDK and place in asctec_mav_framework/asctec_hl_firmware
    - Available [here](http://wiki.asctec.de/display/AR/SDK+Downloads)
    - *Note:* Should download the latest files and host on Git

### Instructions

1. cd to the `asctec_mav_framework/asctec_hl_firmware`
2. Run `catkin_make` or `catkin build` if using [catkin tools](https://github.com/catkin/catkin_tools)
3. Connect the your computer to the AscTec AutoPilot via the JTAG Adapter:

![JTAG adapter](../images/04_01_jtag_adapter.jpg)

4. Check if the device is connected and recognized:
    - `ls /dev/ttyUSB*`
    - There should be one more device then originally
5.  Connect to OpenOCD:
    - `sudo openocd -f lpc2xxx_asctecusbjtag05.cfg`
    - This will log into the internal high level processor
6. If there are any warnings or errors restart the system and start over.
7. Open a telnet to OpenOCD:
    - `telnet localhost 4444`
8. Run the following commands to stop the device, upload the compiled binary, and to restart the device:
    - `reset halt`
    - `flash write_image erase main.bin`
    - `reset run`
9. Restart the Firefly completely.
10. Note that the main LED lights will turn to a constant red. This is normal.
11. If everything is working correctly, you should not get any more rx timeouts when running the asctec ros package
