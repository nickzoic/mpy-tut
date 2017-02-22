==================================
 Installing MicroPython for ESP32
==================================

Download Firmware
=================

Download from github:

https://github.com/nickzoic/micropython-esp32/releases/download/meetup-1/combined.bin


Install esptool & Upload Firmware
=================================

Linux
-----

From a terminal::

    sudo addgroup
    sudo pip install --upgrade https://github.com/espressif/esptool/tarball/v2.0beta2
    esptool.py version

Your serial device is almost certainly /dev/ttyUSB0::

    esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash 0 combined.bin

Mac OSX (10.11)
---------------

From terminal::

    sudo easy_install pip
    sudo pip install --upgrade https://github.com/espressif/esptool/tarball/v2.0beta2
    esptool.py version

Your device is called something like "/dev/cu.*" depending on the type of device::

    ls /dev/cu.*
    esptool.py --port /dev/cu.usbserial --baud 460800 write_flash 0 combined.bin

Windows 10
----------

Install Python 3 from https://www.python.org/downloads/windows/
(Select yes, add it to your path)

Open a command shell (CMD.EXE)::

    pip install --upgrade https://github.com/espressif/esptool/tarball/v2.0beta2

Your device is called COM3 or COM4 or something along those lines

    esptool.py --port COM3 --baud 460800 write_flash 0 combined.bin


Connecting to REPL
==================

Use 'miniterm.py' which is installed at the same time as esptool.  Use the port name
you worked out in the previous step

    miniterm.py /dev/ttyUSB0 115200
    miniterm.py /dev/cu.mumble 115200
    miniterm.py COM4 115200

You can now chat to Python at the REPL






