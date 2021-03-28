# pyupdi
Python UPDI driver for programming "new" tinyAVR and megaAVR devices

Written by [mraardvark](https://github.com/mraardvark/pyupdi) - forked by [braindead](https://github.com/braindead-sec) to add a memory dump feature

pyupdi is a Python utility for programming AVR devices with UPDI interface
  using a standard TTL serial port.

  Connect RX and TX together with a suitable resistor and connect this node
  to the UPDI pin of the AVR device.

  Be sure to connect a common ground, and use a TTL serial adapter running at
   the same voltage as the AVR device.

<pre>
                        Vcc                     Vcc
                        +-+                     +-+
                         |                       |
 +---------------------+ |                       | +--------------------+
 | Serial port         +-+                       +-+  AVR device        |
 |                     |      +----------+         |                    |
 |                  TX +------+   4k7    +---------+ UPDI               |
 |                     |      +----------+    |    |                    |
 |                     |                      |    |                    |
 |                  RX +----------------------+    |                    |
 |                     |                           |                    |
 |                     +--+                     +--+                    |
 +---------------------+  |                     |  +--------------------+
                         +-+                   +-+
                         GND                   GND

</pre>
When running pyupdi on Raspberry pi, use GPIOs 14 and 15 for UART TX and RX.
On rpi3, be sure to apply the device tree overlay to map UART0/ttyAMA0 to these pins (relocate or disable Bluetooth device).
More information here: https://www.raspberrypi.org/documentation/configuration/uart.md

## install
```
git clone https://github.com/braindead-sec/pyupdi.git
cd pyupdi
python3 setup.py
```

## usage
```
$ pyupdi -h
usage: pyupdi [-h] -d
              {atmega1608,atmega1609,atmega3208,atmega3209,atmega4808,atmega4809,atmega808,atmega809,attiny1604,attiny1606,attiny1607,attiny1614,attiny1616,attiny1617,attiny202,attiny204,attiny212,attiny214,attiny3216,attiny3217,attiny402,attiny404,attiny406,attiny412,attiny414,attiny416,attiny417,attiny804,attiny806,attiny807,attiny814,attiny816,attiny817,avr128da28,avr128da32,avr128da48,avr128da64,avr128db28,avr128db32,avr128db48,avr128db64,avr16dd14,avr16dd20,avr16dd28,avr16dd32,avr32da28,avr32da32,avr32da48,avr32db28,avr32db32,avr32db48,avr32dd14,avr32dd20,avr32dd28,avr32dd32,avr64da28,avr64da32,avr64da48,avr64da64,avr64db28,avr64db32,avr64db48,avr64db64,avr64dd14,avr64dd20,avr64dd28,avr64dd32,mega1608,mega1609,mega3208,mega3209,mega4808,mega4809,mega808,mega809,tiny1604,tiny1606,tiny1607,tiny1614,tiny1616,tiny1617,tiny202,tiny204,tiny212,tiny214,tiny3216,tiny3217,tiny402,tiny404,tiny406,tiny412,tiny414,tiny416,tiny417,tiny804,tiny806,tiny807,tiny814,tiny816,tiny817}
              -c COMPORT [-e] [-b BAUDRATE] [-f FLASH] [-du DUMP] [-r] [-i] [-fs [FUSES ...]]
              [-fr] [-v]

Simple command line interface for UPDI programming

optional arguments:
  -h, --help            show this help message and exit
  -d {atmega1608,atmega1609,atmega3208,atmega3209,atmega4808,atmega4809,atmega808,atmega809,attiny1604,attiny1606,attiny1607,attiny1614,attiny1616,attiny1617,attiny202,attiny204,attiny212,attiny214,attiny3216,attiny3217,attiny402,attiny404,attiny406,attiny412,attiny414,attiny416,attiny417,attiny804,attiny806,attiny807,attiny814,attiny816,attiny817,avr128da28,avr128da32,avr128da48,avr128da64,avr128db28,avr128db32,avr128db48,avr128db64,avr16dd14,avr16dd20,avr16dd28,avr16dd32,avr32da28,avr32da32,avr32da48,avr32db28,avr32db32,avr32db48,avr32dd14,avr32dd20,avr32dd28,avr32dd32,avr64da28,avr64da32,avr64da48,avr64da64,avr64db28,avr64db32,avr64db48,avr64db64,avr64dd14,avr64dd20,avr64dd28,avr64dd32,mega1608,mega1609,mega3208,mega3209,mega4808,mega4809,mega808,mega809,tiny1604,tiny1606,tiny1607,tiny1614,tiny1616,tiny1617,tiny202,tiny204,tiny212,tiny214,tiny3216,tiny3217,tiny402,tiny404,tiny406,tiny412,tiny414,tiny416,tiny417,tiny804,tiny806,tiny807,tiny814,tiny816,tiny817}, --device {atmega1608,atmega1609,atmega3208,atmega3209,atmega4808,atmega4809,atmega808,atmega809,attiny1604,attiny1606,attiny1607,attiny1614,attiny1616,attiny1617,attiny202,attiny204,attiny212,attiny214,attiny3216,attiny3217,attiny402,attiny404,attiny406,attiny412,attiny414,attiny416,attiny417,attiny804,attiny806,attiny807,attiny814,attiny816,attiny817,avr128da28,avr128da32,avr128da48,avr128da64,avr128db28,avr128db32,avr128db48,avr128db64,avr16dd14,avr16dd20,avr16dd28,avr16dd32,avr32da28,avr32da32,avr32da48,avr32db28,avr32db32,avr32db48,avr32dd14,avr32dd20,avr32dd28,avr32dd32,avr64da28,avr64da32,avr64da48,avr64da64,avr64db28,avr64db32,avr64db48,avr64db64,avr64dd14,avr64dd20,avr64dd28,avr64dd32,mega1608,mega1609,mega3208,mega3209,mega4808,mega4809,mega808,mega809,tiny1604,tiny1606,tiny1607,tiny1614,tiny1616,tiny1617,tiny202,tiny204,tiny212,tiny214,tiny3216,tiny3217,tiny402,tiny404,tiny406,tiny412,tiny414,tiny416,tiny417,tiny804,tiny806,tiny807,tiny814,tiny816,tiny817}
                        Target device
  -c COMPORT, --comport COMPORT
                        Com port to use (Windows: COMx | *nix: /dev/ttyX)
  -e, --erase           Perform a chip erase (implied with --flash)
  -b BAUDRATE, --baudrate BAUDRATE
  -f FLASH, --flash FLASH
                        Intel HEX file to flash.
  -du DUMP, --dump DUMP
                        Dump flash memory to binary file.
  -r, --reset           Reset
  -i, --info            Info
  -fs [FUSES ...], --fuses [FUSES ...]
                        Fuse to set (syntax: fuse_nr:0xvalue)
  -fr, --readfuses      Read out the fuse-bits
  -v, --verbose         Set verbose mode
```
