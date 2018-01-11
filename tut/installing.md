# Installing MicroPython for ESP8266 & ESP32

## Download Firmware

Download prebuilt firmware from micropython.org:

* [MicroPython Firmware for ESP8266 boards](http://micropython.org/downloads#esp8266)
* [MicroPython Firmware for ESP32 boards](http://micropython.org/downloads#esp32)

Or if you cloned this repo, it's already here:

*  [ESP8266](bin/esp8266-20180104-v1.9.3-238-g42c4dd09.bin)
*  [ESP32](bin/esp32-20180104-v1.9.3-238-g42c4dd09.bin)

## Install esptool

You need at least Version 2 of the esptool utility.

### Linux

Note that the vendor package esptool is horribly out of date on Ubuntu at least,
so you're better off installing from pypi.  The vendor package is generally installed
as `/usr/bin/esptool`, whereas the pypi version is generally installed as 
`/usr/local/bin/esptool.py`.  

From a terminal, first add yourself to the group 'dialout':

    sudo addgroup $USER dialout
    exec newgrp dialout
    exec newgrp -

And then install esptool:

    sudo pip install esptool
    esptool.py version

Your serial device is almost certainly /dev/ttyUSB0 or /dev/ttyACM0 or something like that.
When you find it, set up an environment variable so we don't have to keep typing it:

    export PORT=/dev/ttyUSB0

### Mac OSX (10.11)

From terminal::

    sudo easy_install pip
    sudo pip install --upgrade esptool
    esptool.py version

Your device is called something like "/dev/cu.something" depending on the type of device.
When you find it, set up an environment variable so we don't have to keep typing it:

    export PORT=/dev/cu.usbserial

If you can't find any devices which look like that, you need to install third-party
"VCP" (Virtual COM Port) drivers for your device.  Typical locations:

* [FTDI VCP Drivers](http://www.ftdichip.com/Drivers/VCP.htm)
* [Silicon Labs VCP Drivers](http://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)

### Windows 10

Install Python 3 from https://www.python.org/downloads/windows/
(Select yes, add it to your path)

Open a command shell (CMD.EXE, or bash subsystem):

    pip install --upgrade esptool

Your device is called COM3 or COM4 or something along those lines.
When you find it, set up an environment variable so we don't have to keep typing it:

    set PORT=COM3

    esptool.py --port COM3 --baud 460800 write_flash 0 combined.bin

If you can't find any devices which look like that, you need to install third-party
"VCP" (Virtual COM Port) drivers for your device.  Typical locations:

* [FTDI VCP Drivers](http://www.ftdichip.com/Drivers/VCP.htm)
* [Silicon Labs VCP Drivers](http://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)

## Writing Firmware

For esp8266:

    esptool.py --port $PORT --baud 115200 write_flash 0 bin/esp8266-20180104-v1.9.3-238-g42c4dd09.bin

For esp32 (note: the offset is different):

    esptool.py --port $PORT --baud 115200 write_flash 0x1000 bin/esp32-20180104-v1.9.3-238-g42c4dd09.bin


## Connecting to REPL

Use 'miniterm.py' which is part of pyserial, and installed at the same time as esptool.
Use the port name you worked out in the previous step

    miniterm.py $PORT 115200

You can now chat to Python at the REPL.

## Setting up WiFi

There's plenty of other microcontrollers around: the thing which makes the ESP microcontrollers
a bit special is their built-in support for WiFi networks.

Micropython exposes this via the 'network' library.  To get your device talking on the network,
do the following

    import network
    w = network.WLAN()
    w.active(True)
    w.connect('AP','Password')
    w.ifconfig()

That last command returns a tuple of (IP address, netmask, gateway address, DNS address).
Note down your IP address ... we'll use it later.

# EXERCISES

* Load the latest MicroPython firmware onto your device.

* Configure it onto the conference network (details TBD)

* Check that you can ping it from your laptop.  You may need to reconnect your
  laptop to the same SSID / Password as you used for the device. 

* Assuming you're using an ESP8266 device, set up WebREPL as per the 
  [WebREPL and WebPad](webrepl-and-webpad.md) page.

