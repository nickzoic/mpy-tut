# MicroPython: Networking

There's plenty of other microcontrollers around: the thing which makes the ESP microcontrollers
a bit special is their built-in support for WiFi networks.

Micropython exposes this via the 'network' library.

## Configuring WiFi

The main conference network is on 5GHz, so a special workshop network has been provisioned 
for this tutorial.  Details TBA.

To set up wifi:

    import network
    w = network.WLAN()
    w.active(True)
    w.connect('AP','Password')
    w.ifconfig()

That last command returns a tuple of (IP address, netmask, gateway address, DNS address).
Note down your IP address ... we'll use it when we connect between devices.

Check that you can ping the IP address from your laptop.  You may need to reconnect your
laptop to the same SSID / Password as you used for the device.

## A simple TCP/IP connection

Once you've got a wifi connection, you can use the 'socket' library to make a connection
to another machine:

    import socket
    sock = socket.socket()
    sock.connect(('example.com', 80))
    sock.write('GET / HTTP/1.0\nHost: example.com\n\n')
    sock.readline()

Prints:

    b'HTTP/1.0 200 OK\r\n'

You can also listen for inbound TCP connections, for example this FTP server:

    import socket
    sock1 = socket.socket()
    sock1.bind(('0.0.0.0', 21))
    sock1.listen(1)
    while True:
        sock2, remote = sock1.accept()
        sock2.write(b"220 Service Ready\r\n")
        print(sock2.readline())
        sock2.write(b"421 Only kidding!\r\n")
        sock2.close()

## Libraries

Writing our own client and server every time would be tedious at best,
and protocols such as HTTP are not as simple as they may at first seem.
Let's not do that.  There are several options for libraries.

Check out [Libraries](libraries.html) for details of how to install these,
but for the moment I've built images for the ESP8266 and ESP32 which
have a selection of libraries included.

### HTTP Client: requests

There's a small subset of the requests library available as 
'micropython-urequests' which implements part of HTTP.  There's a much 
more useful and complete library 'micropython-http.client' which is usable on the
ESP32 but it pulls in too many dependencies to be useful on the ESP8266.

### HTTP Server: picoweb

Picoweb is a small web server on top of uasyncio.  It is at https://github.com/pfalcon/picoweb/

There's also a few efforts to put web server frameworks with purely asyncronous 
operations: perhaps have a look at https://github.com/nickzoic/pycose but it's a work
in progress.

### MQTT Client: umqtt.simple

MQTT is a simple message queueing protocol which is a good match for the limited
resources available to IoT devices.

There's a public server available at iot.eclipse.org or you can run your own using
packages such as mosquitto.

    import machine
    from umqtt.simple import MQTTClient

    mc = MQTTClient(machine.unique_id(), "iot.eclipse.org")
    mc.connect()
    while True:
        mc.publish("mpytut", "hello from micropython!")

# Exercises

1. Connect up to the WiFi, and copy umqtt.simple library onto your device.

2. Send some messages, and check they're arriving at the mosquitto subscriber

3. Write a MQTT subscriber which receives messages from the broker and does
   something interesting.  You can either use mosquitto\_pub to send messages,
   or team up with someone else to communicate between your devices. 
