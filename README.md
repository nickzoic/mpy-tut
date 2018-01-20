# MPY-TUT

This is the notes for the upcoming
[Getting Started with MicroPython Tutorial at LinuxConfAU 2018](https://rego.linux.conf.au/schedule/presentation/42/)
which aims to introduce people to the very basics of [MicroPython](https://micropython.org/) in an easy and accessible way.

It is based on material developed at [MicroPython Meetup](https://www.meetup.com/MicroPython-Meetup/).
This content is written by [Nick Moore](http://nick.zoic.org/) and licensed [CC0](https://creativecommons.org/publicdomain/zero/1.0/)
Please feel free to use it for whatever purpose.

## Running Sheet

(approximate)

Time  | Activity
------|-----------------------------------------------------
10:45 | Introduction (Nick Moore / Damien George)
10:55 | Hands on: Installing, the REPL, I/O
11:30 | Short Break
11:40 | Hands on: Networks, Libraries, Building from source
12:15 | Developing on MicroPython (Nick Moore)
12:25 | Lunch / More hacking


## The tutorial: what you need to know

The examples apply as far as possible to both the ESP8266 and the ESP32 ports of MicroPython.  

I'll have a limited number of ESP8266-based "Witty Cloud" boards along at the tutorial.
You can purchase one of these for $10 as part of your LinuxConfAU registration
([under "Extras"](https://rego.linux.conf.au/tickets/category/7)).  If there's any 
still available after ticket sales close then it's first-come-first-served cash only
at the start of the tutorial.

*Please provide your own MicroUSB cable (surely everyone has a pile of these?) and any
dongles your computer needs to talk to it!*

The [Open Hardware MiniConf](http://www.openhardwareconf.org/wiki/OHMC2018) will be
constructing [LoliBot](https://github.com/CCHS-Melbourne/LoliBot)
robots based on the ESP32 and capable of running MicroPython, so if you're going to that
as well bring yours along.

If you have your own ESP8266 or ESP32 development board to bring along, perhaps
from the [PyConAU MicroPython Sprint](http://nick.zoic.org/art/micropython-sprints-pyconau/)
or from one of the many vendors out there, you're welcome to bring that along.

## Before the tutorial

### If you just want to try it out

Clone this repo, install Python 3.6 and esptool (see [Installing](installing.md)),
if you have your own hardware confirm that you can see the serial device at least.

### If you want to build MicroPython from source

As above, plus read [Building](building.md) and follow along until you can't follow any 
more.  Downloading the toolchain and SDKs ahead of time will save a lot of time on
the day.

## Specific Hardware

If you picked up a Witty Cloud module for the tutorial, start here:

* [the Witty Cloud (ESP8266)](tut/witty-cloud.md)

If you were at the Open Hardware MiniConf and got a LoliBot, start here:

* [MicroPython for OHMC LoliBot](tut/ohmc-lolibot.md)

Otherwise, there's more general stuff in the following sections ...

### Installing

* [Installing for ESP8266 & ESP32](tut/installing.md)
* [WebREPL and WebPad (ESP8266 only)](tut/webrepl-and-webpad.md)
* [MicroPython Libraries](tut/libraries.md)

### Inputs and Outputs

* [Input and Output](tut/input-and-output.md)
* [Motors](tut/motors.md)

### Networking

* [Networking](tut/network.md)

### Building MicroPython

* [Building](tut/building.md)

## Resources

* There's a tutorial in the official MicroPython / ESP8266 docs at
  https://docs.micropython.org/en/latest/esp8266/esp8266/tutorial/index.html

* [Nick's slides](http://nick.zoic.org/talk/lca2018/getting-started-with-micropython/)
