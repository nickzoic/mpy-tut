# MicroPython: Motor Control

## DC motors 

See also: [Wikipedia: Brushed DC Motors](https://en.wikipedia.org/wiki/Brushed_DC_electric_motor)

DC motors turn when there's a voltage across them.  But they need more current than our
IO Pins can supply, so we need a driver to amplify the signals from the MCU. This could
be as simple as a single transistor switched from an I/O pin:

![A DC Motor](img/dcmotor.png)

Then you can turn the motor on and off using the pin::

    pin_motor = machine.Pin(4, machine.Pin.OUT)

The motor can be driven at different speeds by varying the duty cycle, just like with the
LED::

    pin_motor = machine.Pin(4, machine.Pin.OUT)
    pwm_motor = machine.PWM(pin_motor)

The motor can also be driven backwards by reversing the direction of the voltage 
across it.  This is done using an [H Bridge](https://en.wikipedia.org/wiki/H_bridge),
which consists of four switches coordinated to allow the motor voltage to be reversed.

![H Bridge](img/hbridge.gif)

Because switching the wrong thing at the wrong time could cause a short circuit, the
switches of an H-bridge are generally controlled by internal circuitry, and driven by
two logic pins.  There's two common ways: one controls the sides of the bridge (often
called "A" and "B") directly:

    pin_motor_a = machine.Pin(4, machine.Pin.OUT)
    pin_motor_b = machine.Pin(5, machine.Pin.OUT)

pin\_motor\_a | pin\_motor\_b | motor direction
--------------|---------------|----------------
False         | False         | Stopped
False         | True          | Clockwise
True          | False         | Counterclockwise
True          | True          | Stopped

... and the other common method uses one pin as a PWM input (to set motor 
speed) and the other as a digit input (to set direction):

    pin_motor = machine.Pin(4, machine.Pin.OUT)
    pwm_motor = machine.PWM(pin_motor)

    pin_reverse = machine.Pin(5, machine.Pin.OUT) 

Appropriate frequencies to PWM motors at are best determined by experimentation, as
they vary between drivers and motors.

## Servos

[Servos](https://en.wikipedia.org/wiki/Servo_(radio_control))
are very handy little units, consisting of a motor, a position sensor and a feedback
loop.  Rather than telling them which way to turn, you tell them what position you want them
to be in and they move to that position.

They are controlled by a train of pulses, for most
servos a pulse of 1.0 ms will turn the servo one way and a pulse of 2.0 ms will turn it the
other.  A pulse of 1.5 ms will put the servo in the middle.  Pulses must be received every
25 ms or so or the servo will turn off.  Servos are not all that precise, especially cheap
ones, so if you go past the acceptable range for the servo you may hear it whine as it tries
to move past its limits, or it may 'hunt' (wiggle back and forth) if it isn't happy with
the frequency of the pulses.  The allowable range of your servos is best determined by trial
and error.

There are three pins:

Wire color | Purpose | NodeMCU Pin
-----------|---------|------------
Brown      | Ground  | GND
Red        | Power   | Vin
Orange     | Signal  | D4

Thankfully this is easy enough to do with the PWM control.  Set the frequency to 50Hz (one
cycle per 20ms) and the duty value to between 51 (51/1023 * 20ms = 1ms) and 102 (102/1023 * 20ms = 2ms)
We can adapt the LED PWM code above::

    import machine
    import time

    pin = machine.Pin(2, machine.Pin.OUT)
    pwm = machine.PWM(pin)
    pwm.freq(50)

    while True:
        pwm.duty(51)
        time.sleep(1)
        for d in range(52,103):
            pwm.duty(d)
            time.sleep(0.2)
        time.sleep(1)


## Stepper Motors

Stepper motors have multiple separate coils, and unlike DC motors there's no brushes to switch
the current around and keep things spinning, instead you have to do it yourself.  The two
separate phases need to be controlled separately.

For more details: [Wikipedia: Stepper Motors](https://en.wikipedia.org/wiki/Stepper_motor)

A typical bipolar stepper motor has two windings A and B which are driven by two
separate H-bridges for a total of four I/O pins.  To make the motor rotate, it must be
driven in the correct sequence, for example here is the sequence for a "half step":

Phase | A+ | A- | B+ | B-
------|----|----|----|----
0     | 1  | 0  | 0  | 0
1     | 1  | 0  | 1  | 0
2     | 0  | 0  | 1  | 0
3     | 0  | 1  | 1  | 0
4     | 0  | 1  | 0  | 0
5     | 0  | 1  | 0  | 1
6     | 0  | 0  | 0  | 1
7     | 1  | 0  | 0  | 1

This means you have more work to do, but you also have more control:

    import machine
    import time

    pins = [
        machine.Pin(12, machine.Pin.OUT),  # 1
        machine.Pin(13, machine.Pin.OUT),  # 2
        machine.Pin(14, machine.Pin.OUT),  # 4
        machine.Pin(15, machine.Pin.OUT),  # 8
    ]

    phases = [ 1, 5, 4, 6, 2, 10, 8, 9 ]

    while True:
        for phase in phases:
            for n, p in enumerate(pins):
                pins[n](phase & 1<<n)
            time.sleep(0.001)

