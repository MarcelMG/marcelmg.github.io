---
layout: post
title: BLEthingy part 2 - software
---
<script src="http://api.html5media.info/1.1.8/html5media.min.js"></script>

In this post I will talk about the software part of my recent project *BLEthingy*. All the software for the microcontroller was written in C using ATMEL Studio. At first, I wrote some code to use and test some features of the microcontroller, like the UART (which is needed to communicate with the BLE module), the internal Real Time Clock (RTC) which can periodically wake up the microcontroller from sleep and the SPI peripheral needed to cummunicate with the accelerometer. Since the UART was already in use connected to the RN4871 module, I used a simple software UART transmitter to send messages to the PC for debugging.


The RN4871 module can be configured with commands sent via UART. [Here](https://github.com/MarcelMG/BLE_thingy/tree/master/software/Beacon_test) is an example that shows how to configure the module as a non-connectible BLE Beacon using sleep mode. This mode allows only to transmit the ID of the Beacon plus some additional data bytes. The advantage is, that no permanent connection (pairing) is needed and by varying the advertising (i.e. transmission) delay, we can achieve a very low power consumption. The BLE beacon advertisement data packet consist of a preamble (I chose the iBeacon format), the UUID (unique identifier of the device), two 2-byte variables called major and minor and one 1byte value to calibrate the TX-power to approximate the distance to the beacon. In my application I used the major and minor bytes for the data.
[This page](https://os.mbed.com/blog/entry/BLE-Beacons-URIBeacon-AltBeacons-iBeacon/) explains the different types of beacons and their data structure.

To use the ADXL345 I wrote a [library](https://github.com/MarcelMG/ADXL345_lib) for it from scratch. [This example](https://github.com/MarcelMG/BLE_thingy/blob/master/software/low_power_test_1/main.c) shows how to use the accelerometer's activity detection feature to wake up the microcontroller from sleep.


When finally all components worked well, I assembled them into a [demo application](https://github.com/MarcelMG/BLE_thingy/tree/master/software/BLE_beacon_activity_sensor). This application realizes the following features:
* motion detection with accelerometer
* use onboard pushbutton
* temperature measurement via the ATtiny3216's internal temperature sensor
* battery voltage measurement

The advertisement interval is set to 10s, i.e. every 10s an advertisement package is sent via BLE. The microcontroller is woken up by the RTC every 32s to measure the temperature and the battery voltage and to send those values. The accelerometer detects activity (motion) and can then wake up the microcontroller to send a message. So does pressing the pushbutton. The BLE RF-module is also in sleep mode unless woken up by the microcontroller. The average current consumption is approx. **190µA** with the above settings (intervals). During BLE transmission and when the LED is on, the maximum current is about 14mA. The current was measured with the oscilloscope across a 10 Ohm shunt resistor. This amounts to a battery life of roughly 1 month with the CR2032 coin cell battery.

Now, the data sent by the *BLEthingy* has to be received and processed for it to be useful. During the development for testing I used various smartphone apps for BLE and BLE beacons for debugging. For the final demo application, I wanted to process the data on the PC. Therefore I used a Bluetooth USB Dongle that supports BLE. I found a very cheap (4€) one labeled "CSR V4.0" that is supported also under Linux. You have to verify that it uses the CSR8510 (A10) chipset. First, it didn't work for me under Ubuntu. Then I found the solution in this [blog comment](http://blog.ruecker.fi/2013/10/06/adventures-in-bluetooth-4-0-part-i/#comment-318). You have to use a software called "BlueSuite" under Windows to configure the dongle in another mode (only once). Then it works fine under Linux.

To receive the data advertised by the BLE Beacon, I used the command-line tool *hcidump* to get the raw data. Then I wrote a small C++ program which reads in the raw data, extracts and parses the meaningful parts. Then it prints out the temperature and voltage and indicates if an event (motion or buttonpress) has happened. The program is exectuted with a script, which launches the BLE scan and pipes the raw data to the C++ program. The C++ program and the script can be found [here](https://github.com/MarcelMG/BLE_thingy/tree/master/software/BLE_Beacon_PC_application).

**Videos of the demo application**
<video src="https://github.com/MarcelMG/BLE_thingy/raw/master/software/BLE_Beacon_PC_application/motion_detect.mp4" width="480" height="320" controls preload></video>
Motion detection
<video src="https://github.com/MarcelMG/BLE_thingy/raw/master/software/BLE_Beacon_PC_application/button_press.mp4" width="480" height="320" controls preload></video>
Button press


This demo shows what can be done with the *BLEthingy*. One application would be e.g. to monitor if something has been moved, or a person fell or something similar. Using the free pins available on the PCB, additional sensors can be connected, e.g. to monitor environmental parameters and use the device as a sensor node. *BLEthingy* can be considered as a platform for many possible applications. 








