
# WebREPL and WebPad

Unfortunately, this only works in ESP8266 at this point.
WebREPL support for ESP32 is still in progress.

## WebREPL

WebREPL works by opening a WebSocket connection from your
browser through to the REPL.  It's kind of teed in there so 
you'll see the text also appearing in the serial REPL.

1. You can use WebREPL code direct from http://micropython.org/webrepl/

   Or download it using wget:

```
    wget -r -nH http://micropython.org/webrepl/
```

2. Connect to the serial REPL, set up WiFi and work out 
   your new IP address, then set up WebREPL:

```
    import network
    w = network.WLAN()
    w.active(True)
    w.scan()
    w.config("yourAP", "yourPassword")
    w.ifconfig()

    import webrepl_setup
```

   Alternatively, if your device is set up as an AP, you can connect to it
   and point WebREPL at ws://192.168.4.1:8266/ and it will hopefully ask
   you for a password, which it will use to connect from then on.

3. WebREPL lets you type directly to the REPL but also upload files
   to the filesystem

## WebPad

WebPad takes a slightly different approach, kind of like iPython for
WebREPL.  Either:

    git clone https://github.com/nickzoic/mpy-webpad/

or just download it (there's only one file):

    wget https://raw.githubusercontent.com/nickzoic/mpy-webpad/master/webpad.html

Get WebREPL working first, then point WebPad at your IP address and connect up.
There's instructions included, but the point is that it lets you go back
and edit the stuff you previously did.  Early days yet, but let me know 
what you think.


