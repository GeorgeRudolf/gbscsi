# Programming Instructions:

Unfortunately for us, the microcontroller on the board comes basically empty from factory, and JLC
can’t program it for us. To be able to easily load code into it, we need a bootloader. You’ll need an ST-
Link (clones are alright, ~$3 off amazon) or an USB-TTL serial adapter. There’s many ways to do this,
all extensively covered online.
Instead of repeating all that here, I’ll simply point you towards looking for instructions for flashing an
STM32 bootloader on Maple Mini boards and their clones (such as Bluepills).
A bootloader such as this:
https://github.com/rogerclarkmelbourne/STM32duino-bootloader

And instructions like these:
https://www.visualmicro.com/page/STM32-Flashing-Bootloaders.aspx
Like I said, there’s many methods, and they vary for different OSes. Perhaps in the future I’ll improve
documentation. The DFU pushbutton on the board is the same as BOOT0 on a Bluepill board – BL is
connected to BOOT1. After that, you can easily load whatever firmware you may want on the board –
be that some SCSI disk emulator firmware I haven’t yet finished getting to work but should go in this
repo, something of your own doing, ardSCSIno-stm32 and variants, or simply using the board as
devboard if you ever get tired of SCSI. The schematics are all there.

# Programming with BlueSCSI with an FT232H Breakout using OpenOCD on Linux

To get started, you will need a [https://www.adafruit.com/product/2264](Adafruit FT232H Breakout). If these are not available from Adafruit directly, try a reseller like [https://www.digikey.com/en/products/detail/adafruit-industries-llc/2264/5761217](DigiKey) and [https://www.mouser.com/ProductDetail/Adafruit/2264](Mouser) as they typically have them in stock. There are older versions which are USB-uB and don't have the StemmaQT port, and these work just as well.

You will also need a [https://github.com/erichelgeson/BlueSCSI/releases/tag/v1.1-20220917](.bin file from the BlueSCSI project). The link is to the version usedto write this tutorial, but newer versions may be available.

You will also need OpenOCD installed with FTDI driver support. This should be the case for any release in a modern Linux distribution. For reference, the version used for this guide was 0.11.0 on Arch Linux x86_64.

Next you need to wire the FT232H to the SWD header of the GBSCSI device. First, ensure that the "I2C Mode" switch on the FT232H Breakout is in the "on" position. The SWD protocol, like I2C, has input and output on a single pin but the FTDI drivers want them on separate pins so that switch will connect the two pins together. Then connect the following pins:

| GBSCSI Programming Header | FT232H Breakout |
|---------------------------|-----------------|
| 3.3V                      | 3V              |
| SWD                       | MOSI (D1)       |
| SWCK                      | SCK/SCL (D0)    |
| GND                       | Gnd             |

If you have multiple boards, you may find it easier to attach the headers to a simple [https://www.adafruit.com/product/4173](Male Header Pin Strip) and then you can simply hold that in place for the quick programming process without soldering.

Next, connect the FTDI Breakout to your Linux system and open two terminal windows. In one we will launch the OpenOCD program with the following command line:
```sudo openocd -f /usr/share/openocd/scripts/interface/ftdi/ft232h-module-swd.cfg -f /usr/share/openocd/scripts/target/stm32f1x.cfg```
This tells OpenOCD that we are using the ft232h module in SWD mode and that we are connecting to an stm32f1 series device. If you get timeout errors, that usually means your connection between the FT232H Breakout and the GBSCSI is not correct or intermittent.

In the second window, you will need to run:
```telnet localhost 4444```
This will connect you to the command line interface for OpenOCD. From here you can upload the image to the GBSCSI device with the following command:
```flash write_image erase BlueSCSI-v1.1-20220917-STM32F1.bin 0x08000000```
This command assumes that the BlueSCSI image file is in the same directory where OpenOCD was started from. If successful, the ACTY and STATUS light will start blinking in an alternativly. This will overwrite any other information on the microcontroller flash, so the same process and commands can be used for recovery as well.

Once complete, simply press CTRL+C on the OpenOCD window to complete the session.
