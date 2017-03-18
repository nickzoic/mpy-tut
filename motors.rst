===============================
 MicroPython: Journey Onwards!
===============================

This should work on either ESP8266 or ESP32 MicroPython, but the instructions are
written for ESP8266.  There are minor differences, eg: the numbers of pins and their
capabilities.

Note that if you're using a NodeMCU board, the pin numbers printed on the board
are not the same as the ESP8266 GPIO numbers.
See https://nodemcu.readthedocs.io/en/master/en/modules/gpio/

Output Pins
===========

Digital Outputs
---------------

To control an output pin, you must first configure it.  The `machine` library
makes the pins available to your Python code, and let's you specify how you
want to use that pin.  To configure a pin as a digital output::

     import machine
     pin = machine.Pin(4, machine.Pin.OUT)

On NodeMCU, GPIO4 is connected to an on-board LED, so you should be able to turn
the LED on and off::

     pin(True)
     pin(False)

Of course you can do this in a loop to get 'blinky', the microcontroller equivalent
of 'hello world'::

     import machine
     import time

     pin = machine.Pin(4, machine.Pin.OUT)
     while True:
         pin(True)
         time.sleep(1)
         pin(False)
         time.sleep(1)

PWM Outputs
-----------

You can also turn the LED "partly on" by turning it on and off rapidly.  Doing this
in Python would be flickery and a waste of power, but thankfully there's hardware support
for pulse-width modulation (PWM).  This just means turning the pin on and off rapidly,
and it lets you set the proportion of the time the LED is on, called the 'duty cycle'::

    _/`\___________/`\___________/`\____________ duty cycle 10%

    _/``````\______/``````\______/``````\______/ duty cycle 50%

    _/``````````\__/``````````\__/```````````\__ duty cycle 90%

To configure a pin as PWM, wrap the `machine.Pin` object in a `machine.PWM` object:

    import machine
    pin = machine.Pin(4, machine.Pin.OUT)
    pwm = machine.PWM(pin)

    pwm.freq(1000)
    pwm.duty(0.5)

This lets you fade the LED in and out like so::

     import machine
     import time

     pin = machine.Pin(4, machine.Pin.OUT)
     pwm = machine.PWM(pin)
     pwm.freq(1000)

     while True:
         for d in range(10,90,10):
             pwm.duty(d/100)
             time.sleep(0.1)
         for d in range(90,10,-10):
             pwm.duty(d/100)
             time.sleep(0.1)

Yay, it's 'throbby', the microcontroller equivalent of "Hello, World!\n".

Digital Inputs
--------------

The NodeMCU also has a button, attached to GPIO0::

    import machine

    pin = machine.Pin(0, machine.Pin.IN)
    while True:
        if pin.value(): print "True"
        else: print "False" 
        
Analog Inputs
-------------

There's also an analog input pin, sadly only one on ESP8266::

    import machine

    pin = machine.ADC(0)
    while True:
        print pin.value()

Controlling Hardware
====================

DC motors 
---------

DC motors turn when there's a voltage across them.  But they need more current than our
IO Pins can supply, so we need a driver to amplify the signals from the MCU::

    pin_motor = machine.Pin(4, machine.Pin.OUT)

The motor can be driven at different speeds by varying the duty cycle, just like with the
LED::

    pin_motor = machine.Pin(4, machine.Pin.OUT)
    pwm_motor = machine.PWM(pin_motor)

The motor can also be driven backwards by reversing the direction.  Internally the driver
uses an H-Bridge to do this, but all we need to know is that it has a reverse pin::

    pin_motor = machine.Pin(4, machine.Pin.OUT)
    pwm_motor = machine.PWM(pin_motor)

    pin_reverse = machine.Pin(5, machine.Pin.OUT) 

Servos
------

Servos are very handy little units, consisting of a motor, a position sensor and a feedback
loop.  Rather than telling them which way to turn, you tell them what position you want them
to be in and they move to that position.  They are controlled by a train of pulses, for most
servos a pulse of 1.0 ms will turn the servo one way and a pulse of 2.0 ms will turn it the
other.  A pulse of 1.5 ms will put the servo in the middle.  Pulses must be received every
25 ms or so or the servo will turn off.  Servos are not all that precise, especially cheap
ones, so if you go past the acceptable range for the servo you may hear it whine as it tries
to move past its limits, or it may 'hunt' (wiggle back and forth) if it isn't happy with
the frequency of the pulses.

Thankfully this is easy enough to do with the PWM control.  Set the frequency to 50Hz (one
cycle per 20ms) and the duty cycle to between 0.05 (20ms * 0.05 = 1ms) and 0.10 (20ms * 0.10 = 2ms)
We can adapt the LED PWM code above::

    import machine
    import time

    pin = machine.Pin(4, machine.Pin.OUT)
    pwm = machine.PWM(pin)
    pwm.freq(50)

    while True:
        for d in range(5,10):
            pwm.duty(d/100)
            time.sleep(0.1)

Stepper Motors
--------------

Stepper motors have multiple separate coils, and unlike DC motors there's no brushes to switch
the current around and keep things spinning, instead you have to do it yourself.  This
means you have more work to do, but you also have more control::

    import machine

    pins = [
        machine.Pin(4, machine.Pin.OUT)  # 1
        machine.Pin(5, machine.Pin.OUT)  # 2
        machine.Pin(6, machine.Pin.OUT)  # 4
        machine.Pin(7, machine.Pin.OUT)  # 8
    ]

    phases = [ 1, 5, 4, 6, 2, 10, 8, 9 ]

    while True:
        for phase in phases:
            for n, p in enumerate(pins):
                pins[n](phase & 1<<n)


