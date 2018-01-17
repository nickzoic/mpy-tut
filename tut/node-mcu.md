### NodeMCU (ESP8266)

Note that if you're using a NodeMCU board, the pin numbers printed on the board
are not the same as the ESP8266 GPIO numbers.
See https://nodemcu.readthedocs.io/en/master/en/modules/gpio/

NodeMCU | ESP8266 / MicroPython | Notes
--------|-----------------------|---------------
D0      | GPIO16                | Limited features, LED on NodeMCU
D1      | GPIO5                 |
D2      | GPIO4                 |
D3      | GPIO0                 | Connected to button
D4      | GPIO2                 | Connected to LED on module
D5      | GPIO14                |
D6      | GPIO12                |
D7      | GPIO13                |
D8      | GPIO15                |
D9      | GPIO3                 | UART RXD0 (used for console)
D10     | GPIO1                 | UART TXD0 (used for console)
D11     | GPIO9                 | (used for module flash memory)
D12     | GPIO10                | (used for module flash memory)

