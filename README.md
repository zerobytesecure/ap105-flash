# Flash Tool for Aruba AP-105

Tool to exchange Aruba AP-105 boot loader stored on a MX25L12835 flash chip with uboot for flashing OpenWRT with an RaspberryPi.

## Prepare

Requirements:

- RaspberryPi
- Programming clip SOIC16/SOP16 and cabling
- [Raspbian](https://www.raspberrypi.com/software/)
- [enabled SPI interface](https://codingworld.io/spi-aktivieren-am-raspberry-pi/)
- [`flashrom`](https://www.flashrom.org/Flashrom) tool

Configure SPI on the RaspberryPi:

```
sudo raspi-config
# Select Interfacing Options
# Select SPI 
# Select Yes
# Select Ok
# Leave with finish or ESC
```

To install `flashrom`:

```
sudo apt-get install flashrom
```

Connect the AP105 Flash chip. The red status LED on AP105 must glow

```
        *-------------------------------------*
        |                                     |
        |          ------|MX25L12835------    |
        |         |                       |   |
        |   *-----|1 HOLD#/SIO3    CLK 16 |---|----*
        |   *-----|2 VCC       SI/SIO0 15 |---*    |
        |   |     |3 RESET#               |        |
        |   |     |4                      |        |
        |   |     |5                      |        |
        |   |     |6                      |        |
        |   |  *--|7 CS#           GND 10 |------* |
        | *-|-----|8 SO/SIO1     WP#/SIO2 |-*    | |
        | | |  |   -----------------------  |    | |
        | | |  |                            |    | | 
        | | *-------------------------------*    | |
        | | |  |                                 | |
        | | |  *-------------------------------* | |
        | | |                                  | | |
        | | |    --------|RaspberryPi|-------  | | |
        | | |   | 1                         2| | | |
        | | |   | 3                         4| | | |
        | | |   | 5                         6| | | |
        | | |   | 7                         8| | | |
        | | |   | 9                        10| | | |
        | | |   |11                        12| | | |
        | | |   |13                        14| | | |
        | | |   |15                        16| | | |
        | | *---|17 +3V3                   18| | | |
        | *-----|19 MOSI/GPIO              20| | | |
        *-------|21 MISO/GPIO              22| | | |
          *-----|23 SCLK/GPIO  GPIO 8/CE0# 24|-* | |
          | *---|25 GND                    26|   | |
          | |   |27                        28|   | |
          | |   |29                        30|   | |
          | |   |31                        32|   | |
          | |   |33                        34|   | |
          | |   |35                        36|   | |
          | |   |37                        38|   | |
          | |   |39                        40|   | |
          | |    ----------------------------    | |
          | |                                    | |
          | *------------------------------------* |
          *----------------------------------------*
```

## Usage

```
ap105-flash <SERIAL NUMBER>
```

The flash will be secured into files:

- `AP105-<SERIAL NUMBER>-dump0.rom`
- `AP105-<SERIAL NUMBER>-dump1.rom`

The `AP105-<SERIAL_NUMBER>-dump0.rom` will be patched with uboot binary `uboot.bin` and the file `AP105-<SERIAL NUMBER>-patch.rom` will be created.

After a confirmation the patched file will be flashed to the device.

## Known problems

### Chip isn't detected by flashrom

- Check the wiring
- Ensure the RaspberryPi have enough power like a power supply with 5V/2A
- Ensure the red LED is illuminated on the AP105
- Reduce the SPI speed to 1000 kHz (default 16500 kHz)

```
SPEED=1000 ap105-flash <SERIAL NUMBER>
```

### flashrom fails during erase

- Check power supply
- Reduce the SPI speed like described above

## Sources

- [git repository for uboot loader @chunkey](https://github.com/chunkeey/u-boot-ap105/releases)
- [OpenWRT device page](https://openwrt.org/toh/aruba/ap-105)
- [Datasheet MX25L12835](https://pdf1.alldatasheet.com/datasheet-pdf/view/575542/MCNIX/MX25L12835E.html)
- [RaspberryPi ASCII Art](http://weyprecht.de/2015/11/30/raspberry-pi-ascii-art/)
- [flashrom page for RaspberryPi](https://www.flashrom.org/RaspberryPi)

