# MicroPython Libraries

Writing everything from scratch would be tedious at best.
Part of the joy of Python is the execellent range libraries
available.  Unfortunately, not all libraries translate well
to the very resource constrained environments MicroPython
runs in, so we have our own selection of packages.

## micropython-lib

There are many basic Python libraries bundled together in the 
[micropython-lib](https://github.com/micropython/micropython-lib/)
repository.  These tend to be cut down versions of the 'real'
libraries, with incomplete APIs.

## upip

Theres packages are also available through `upip`, which is a cut
down version of pip, suitable for the micropython environment.
upip uses the main PyPI repository still, which the packages given
names like 'micropython-\*' to avoid confusion.  Once you're on WiFi,
You can install libraries directly to your device by calling
upip.install from the repl:

    import upip
    upip.install('micropython-uasyncio')

... and packages and dependencies will be installed:

    Installing to: /lib/
    Installing micropython-uasyncio 1.4 from ...
    Installing micropython-uasyncio.core 1.7.1 from ...

Libraries are installed to the device's 'lib' directory.

## modules

Running upip on the device consumes resources you may not have
available, and it's really not convenient
for a bulk rollout of devices anyway.

As an alternative, you can install modules locally and then copy
them into place with mpy-utils, ampy, rshell or webrepl.
To install modules locally, use the 'unix' port of micropython ...

    micropython -m upip install 'micropython-uasyncio'

Or, you can install them into your source tree's modules directory,
where they will be recompiled and built into your flash image.
This is a more effective way to deal with packages on very small
devices like the ESP8266.

(see [Building](tut/building.md) for more information)


