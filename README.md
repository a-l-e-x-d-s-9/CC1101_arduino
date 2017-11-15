# CC1101_arduino
A clone of the ELECHOUSE_CC1101 https://github.com/simonmonk/CC1101_arduino.git.

Changes:
* Initialization with custom pins.
* Whitening turned on by default.
* Support single wire compatible mode, to use RCSwitch and Livolo libraries).


# Connecting an Arduino to a CC1101
These instructions are for an Arduino Uno.

|Arduino|CC1101|Notes|
|-------|------|-----|
|GND    |GND| |
|3.3V|VCC||
|10|CSN/SS|Must be level shifted to 3.3V|
|11|SI/MOSI|Must be level shifted to 3.3V|
|12|SO/MISO||
|13|SCK|Must be level shifted to 3.3V|
|2|GD0|Signals buffer ready to read|


![alt text](https://github.com/a-l-e-x-d-s-9/CC1101_arduino/blob/master/F19_10.png?raw=true_ "Connections")


# Installing the Library

To install the library into your IDE:
* click on the Clone or Download button on this Github page and select Download ZIP.
* Start the Arduino IDE and from the Sketch menu do Sketch->Include Library->Add ZIP Library and select the ZIP you just downloaded.


# API Reference

This is a very easy library to use. You may just wish to try out the examples, that send a text message from one Arduino to another using the Serial Monitor. But for completeness, here it is:


## Include File

```
#include <ELECHOUSE_CC1101.h>
```


## Initialisation

Default pins:
	SCK_PIN    = 13;
	MISO_PIN   = 12;
	MOSI_PIN   = 11;
	SS_PIN     = 10;
	GDO0       = 8;
	
Default frequency: 433 MHz

Choose on of the initialization methods:

```
// 1. Use default pins and frequency
ELECHOUSE_cc1101.Init(); 	

// 2. Set frequency - F_433, F_868, F_965 MHz
ELECHOUSE_cc1101.Init( F_433 );

// 3. Set all pins and frequency: Init( byte freq_pin, byte sck_pin, byte mosi_pin, byte miso_pin, byte ss_pin, byte gd00_pin )
ELECHOUSE_cc1101.Init( F_433, 13, 11, 12, 10, 8 );

```


## Send/Receive Mode

Put this in your setup function and call again, any time after you have processed a received message.

When a new message arives GD0 on the CC1101 (Arduino pin 2) will be set LOW. You can hook this up to an interrupt or just watch for it in your loop function.


```
 ELECHOUSE_cc1101.SetReceive();
```


## Receiving Data

The maximum data size is 64 bits.

ReceiveData requires a buffer of type byte[] and returns the number of bytes contained in the message.

```
int len = ELECHOUSE_cc1101.ReceiveData(buffer);
```



## Sending Data

The maximum data size is 64 bits.

SendData requires a buffer of type byte[] and the number of bytes contained in the message.

```
ELECHOUSE_cc1101.SendData(buffer, len);
```

## Single wire compatible mode
To support libraries that use single wire devices to send and receive RF data, for example: 1. rc-switch. 2. Livolo


```
// Enter single wire compatible mode
ELECHOUSE_cc1101.cc1101_single_wire_start();
 
// Start Tx mode
ELECHOUSE_cc1101.cc1101_single_wire_tx_start();

// End Tx mode
ELECHOUSE_cc1101.cc1101_single_wire_tx_end();
 
 // Enter single wire compatible mode
ELECHOUSE_cc1101.cc1101_single_wire_exit();
 
```


# Legal
MIT license.
