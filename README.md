# MPY-TUT

This is the notes for the upcoming
[Getting Started with MicroPython Tutorial at LinuxConfAU 2018](https://rego.linux.conf.au/schedule/presentation/42/)
which aims to introduce people to the very basics of [MicroPython](https://micropython.org/) in an easy and accessible way.

It is based on material developed at [MicroPython Meetup](https://www.meetup.com/MicroPython-Meetup/).
This content is written by [Nick Moore](http://nick.zoic.org/) and licensed [CC0](https://creativecommons.org/publicdomain/zero/1.0/)
Please feel free to use it for whatever purpose.

## The tutorial: what you need to know

The examples apply to ESP8266 and ESP32 ports of MicroPython.  

The [Open Hardware MiniConf](http://www.openhardwareconf.org/wiki/OHMC2018) will be constructing [LoliBot](https://github.com/CCHS-Melbourne/LoliBot)
robots based on the ESP32 and capable of running MicroPython, so if you're going to that as well bring yours along.

If you have your own ESP8266 or ESP32 development board to bring along, perhaps from the [PyConAU MicroPython Sprint](http://nick.zoic.org/art/micropython-sprints-pyconau/)
or from one of the many vendors out there, you're welcome to bring that along.

I'll also have a limited number of ESP8266-based boards along at the tutorial.  You can purchase one of these for $10 as part
of your LinuxConfAU registration ([under "Extras"](https://rego.linux.conf.au/tickets/category/7).
Please provide your own MicroUSB cable (surely everyone has a pile of these?) and any dongles your computer needs to talk to it!

## Before the tutorial

There's a bunch of stuff to download which would have been a lot quicker if you'd done it earlier, rather than right now just as 
the tutorial is starting.  To save a lot of messing around I've bundled the stuff you need up into a single repository ... this one!
So jump in your time machine and:

1. git clone https://github.com/nickzoic/mpy-tut
2. cd mpy-tut; git submodule update --init --recursive

This downloads a *lot* of content from github.  Most of this is the build environments for ESP32 and ESP8266.
If you're not interested in building MicroPython from source, or you've left your run a little bit late,
you could skip this ... the install instructions include how to install from binaries as well.

## Tutorial Sections

### Installing MicroPython

* [Installing ESP8266 / ESP32](installing.md)
* [WebREPL and WebPad (ESP8266 only)](webrepl-and-webpad.md)

### Inputs & Outputs

* [Input and Output](input-and-output.md)

### Networking

### Multiprocessing

### Reading the Source

## Resources




