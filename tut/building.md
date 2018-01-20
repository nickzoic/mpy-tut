# Building MicroPython for ESP32

## ESP8266

For ESP8266, check out the build instructions at
https://github.com/micropython/micropython/blob/master/ports/esp8266/README.md
and try and follow along.  There's quite a lot to download, especially during the
process of installing https://github.com/pfalcon/esp-open-sdk

Running 'make' on esp-open-sdk will download lots more stuff, so get that under
way before the tutorial.  


## ESP32

For ESP32 the build process is a little simpler, but check out
https://github.com/micropython/micropython/blob/master/ports/esp32/README.md
and follow along.  

The ESP-IDF README.md includes instructions on how to set up on Windows, OSX and 
Linux.  Download the binary xtensa toolchain, and add it to your PATH.
I use & recommend 'direnv' for this. 


