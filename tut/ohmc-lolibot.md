# MicroPython on the OHMC LoliBot

The [Open Hardware MiniConf](http://www.openhardwareconf.org/wiki/OHMC2018)
will be constructing [LoliBot](https://github.com/CCHS-Melbourne/LoliBot)
robots based on the ESP32 and capable of running MicroPython, so if you're
going to that as well bring yours along.

## LoliBot Hardware

The LoliBot is based around a
[LoLin32 Lite](https://wiki.wemos.cc/products:lolin32:lolin32_lite)
which is a simple ESP32 board using an ESP32 chip and 4MB Flash.
It has an onboard Lithium battery controller and voltage regulator and
USB to serial converter.

The ESP32 IO pins are connected as follows

ESP32     | Connection
----------|--------------------
IO2       | Three APA106 Neopixels
IO4       | Reflection Sensor
IO5       | Right Motor A
IO12      | Header J21
IO14      | Header J21
IO13      | Left Motor B
IO15      | Left Motor A
IO16      | "Kick" Servo
IO17      | Interrupt from Accelerometer
IO18      | I2C SDA (to Accelerometer)
IO19      | I2C SCL (to Accelerometer)
IO23      | Right Motor B
IO25      | Header J21
IO26      | Header J21
IO32      | Header J21
IO33      | Header J20
IO34      | Header J20
IO35      | Header J20
IO36 / VP | Header J20
IO37 / VN | Header J20

## Loading Firmware

The first step is to load the MicroPython firmware onto the board.
There's a serial bootloader build into the ESP32 already which means we 
don't need anything fancy, just a USB to serial converter (which is already
built in to the LoliBot)

See [Installing MicroPython](installing.md) for details of how to connect to 
the device and load the MicroPython firmware from Linux, OSX and Windows.

## Exercises: Using I/O

See [Inputs and Outputs](inputs-and-outputs.md) for details of how to control
the I/O pins from MicroPython, and [Motors](motors.sd) for details of how to 
control motors.

The LoliBot has a chain of three Neopixels on pin 2, plus two H-bridge drivers
for the wheel motors (on pins 15/13 and 5/23) and a "kicker" servo on pin 16.

1. Experiment with getting the Neopixels to make interesting colours
   (using the neopixel library)

2. Animate a rainbow effect by setting RGB values, pausing a moment and then changing them.

3. Experiment with driving the wheels in different directions and at different speeds.
   Try to catch it before it runs off the table.

4. Experiment with the servo and how to move it smoothly through its range without
   it hunting or squealing.

5. Detect the presence of an object near the reflection sensor by reading IO4

6. Attach some pieces of wire to pins IO12 and IO14 and experiment
   with the machine.TouchPad interface

