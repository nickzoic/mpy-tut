# MicroPython: Input and Output

This should work on either ESP8266 or ESP32 MicroPython, but the instructions are
written for ESP8266.  There are minor differences, eg: the numbers of pins and their
capabilities.

The number of pins, and their capabilities, varies between ESP8266 and ESP32, and 
not all pins are available on all boards.

## Digital Outputs

To control an output pin, you must first configure it.  The `machine` library
makes the pins available to your Python code, and let's you specify how you
want to use that pin.  To configure a pin as a digital output::

     import machine
     pin = machine.Pin(2, machine.Pin.OUT)

### Example: ESP-12

**Note: On the LoLin32Lite, used in the LoliBot, there's an equivalent blue LED on pin 22**

On the ESP-12 module, GPIO2 is connected to an on-board LED, so you should be able to turn
the LED on and off:

     pin(True)
     pin(False)

Note that it is connected backwards: to turn the LED *on*, set the pin value to False and to 
turn it *off* set the pin value to True.

Of course you can do this in a loop to get 'blinky', the microcontroller equivalent
of `print('hello world')`:

     import machine
     import time

     pin = machine.Pin(2, machine.Pin.OUT)
     while True:
         pin(True)
         time.sleep(1)
         pin(False)
         time.sleep(1)

## PWM Outputs

You can also turn the LED "partly on" by turning it on and off rapidly.  Doing this
in Python would be flickery and a waste of power, but thankfully there's hardware support
for pulse-width modulation (PWM).  This just means turning the pin on and off rapidly,
and it lets you set the proportion of the time the LED is on, called the 'duty cycle':

![Duty Cycle](img/dutycycle.png)

To configure a pin as PWM, wrap the `machine.Pin` object in a `machine.PWM` object:

    import machine
    pin = machine.Pin(2, machine.Pin.OUT)
    pwm = machine.PWM(pin)

    pwm.freq(1000)
    pwm.duty(512)

`freq` sets the frequency (in Hz) and `duty` sets the duty cycle between 0 (always off)
and 1023 (always on).  Beyond about 30Hz, the LED will no longer appear to be flashing,
instead it will be changing in perceived brightness.

This lets you fade the LED in and out like so:

     import machine
     import time

     pin = machine.Pin(2, machine.Pin.OUT)
     pwm = machine.PWM(pin)
     pwm.freq(1000)

     while True:
         for d in range(0,1024,8):
             pwm.duty(d)
             time.sleep(0.01)
         for d in range(1024,0,-8):
             pwm.duty(d)
             time.sleep(0.01)

Yay, it's 'throbby', the microcontroller equivalent of `print("Hello, World!\n")`.

## Digital Inputs

**Note: There isn't an equivalent button on the LoliBot, as the button on the LoLin32Lite
resets the CPU!  But this should work with the reflectance sensor on GPIO4**

Most ESP8266 development boards have a button attached to GPIO0.  This can be used
to put the device into flash mode when it is reset, but once the device has started
it can be used as a general purpose input::

    import machine

    pin = machine.Pin(0, machine.Pin.IN)
    while True:
        if pin(): print "True"
        else: print "False" 
        
## Analog Inputs

**Note: There's isn't really equivalent hardware on the LoliBot, sorry**

There's also an analog input pin, sadly only one on ESP8266::

    import machine

    adc = machine.ADC(0)
    while True:
        print adc.read()

On the ESP32 there are more ADC channels available.  Currently,
8 channels are available on GPIO pins 32 through 39.  The ADC
can be programmed with variable attenuation and resolution, shared
across all the channels.

    import machine

    machine.ADC.atten(machine.ADC.ATTEN_0DB)
    machine.ADC.width(machine.ADC.WIDTH_11BIT)

    adc = machine.ADC(machine.Pin(36))
    while True:
        print adc.read()

## NeoPixels

**Note: On the LoliBot, there's a string of three APA106 NeoPixels on GPIO2.**

"NeoPixels" is a name given to a family of coloured LEDs with an onboard controller.
There's a tiny controller in each pixel, and you can daisy chain them together to
control many pixels from a single output line.  There are several different versions
of these chips, including RGB and RGBW varieties.  Some examples:

* WS2812
* WS2812B
* APA106

To control them from MicroPython, use the `neopixel` library:

    import neopixel
    import machine
    
    pix = neopixel.NeoPixel(machine.Pin(2), 3)
    pix[0] = (255,0,0)
    pix[1] = (0,255,0)
    pix[2] = (0,0,255)
    pix.write()

NeoPixels can be purchased from Ebay (etc) preassembled into ribbons, rings, grids and other shapes.
Controlling a handful of pixels may seem like a silly thing to do when you're used to having millions of
pixels at your disposal, but it can be a lot of fun.

## I2C

**Note: LoliBot features an MPU-9250 accelerometer / gyrometer on pins 18 (SDA)/19 (SCL).
It should work with the code below.**

I2C is a shared serial bus which allows your microcontroller to communicate with multiple 
peripheral devices using a single pin.  

It is part of the `machine` library

    import machine
    import time

    i2c = machine.i2c(freq=400000,scl=machine.Pin(19),sda=machine.Pin(18))
    i2c.writeto_mem(104,107,bytes([0]))
    
    while True:
        print("%6d %6d %6d" % struct.unpack(">3h", i2c.readfrom_mem(104,0x3b,6))
        time.sleep(1)

The I2C library is still quite low level, and using it involves a lot of reading of 
datasheets.  However, it is quite easy to wrap I2C functions into small library functions.
For the [BuzzConf Rocket Surgery](http://nick.zoic.org/art/rocket-surgery-airborne-iot-telemetry-buzzconf)
project [we wrapped this sensor up with a very small library](https://github.com/nodebotsau/water-rocket/blob/master/rocket_package/lib/mpu9250.py)

