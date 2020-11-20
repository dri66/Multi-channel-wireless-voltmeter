# Multi-channel-wireless-voltmeter
A multi-channel wireless voltmeter display using ATTINY402, Arduino nano, NRF24L01+ and 128x32 I2C OLED.
The transmitter is optimised for low power and will run from a battery for weeks or months depending on useage, sample frequency and of course battery capacity.
Range is a few meters to 100m depending on how well the radio module is isolated from the controller, and the power setting.
Developed on Auduino

## Notable features of project
* Pin saving by multiple use of microcontroller pins:
  * SPI chip-enable, LED drive, negative input test mode and switch A/B detection on one pin
  * MISO and analog input on one pin
  * UPDI programming and switch input on one pin (without needing a high-voltage UPDI prgrammer)
* Measuring microcontroller Vdd without using an input pin
* Ultra-low power sleep mode of ATTINY 0 and 1 series
* Very small form factors

## Overview
The display receives voltage data from up to 4 transmitters (one channel each) and shows them on a 128x32 OLED display.

Features:
* 0-20V range, can be configured by resistor selection. 0.01V resolution.
* Positive voltage only. Over voltage and undervoltage (negative) indication. +/-100V input tolerant.
* 1Mohm input impedance, can be configured by resistor selection.
* Minimum and maximum display
* Transmitter low battery warning and battery voltage display. Low battery protection.
* Transmitter auto power-off
* Display modes: all voltages or single channel with more detais

## Transmitter operation
* On and Off buttons. Off is a sleep mode
* Configurable auto power-off period. LED flashes 8 times when powering off.
* Powers off if battery below a configurable voltage
* Can disable auto power-off by long press of On button. LED flashes 4 times.
* Microntoller sums four voltage readings to reduce noise and to improved resolution of the 10-bit ADC
* LED warns if radio IC not present on power-up

## Receiver operation
* Displays version info, radio channel and lost-signal timeout on power-up/ reset
* Then shows Summary display of all available voltage channels
* Each channel reading is blank until data received
* 
A Mode button rotates around display modes:
1. Summary display (up to 4 channels): Row 1: '1 vv.vv*?2 nn.nn*?
                                      Row 2: '3 vv.vv*?4 nn.nn*?
 where vv.vv = voltage. *=battery alert, ? = timeout AKA lost signal alert. 16 pixel rows
 Over voltage shows as -----, under voltage as -0.00, no data is just blank

2. Single channel: Row 1: N vv.vv *   Min
                  Row 2:             mm.mm
                  Row 3:         ?   Max
                  Row 4:  Bat b.bb   mm.mm

 where N=channel, nn.nn = volatage,, mm.mm= min or max voltage, 
 b.bb=sender's Vdd (bat) voltage (appears for a few seconds only to reduce clutter)
 Rows are 8 pixels high in Single view, though N and vv.vv are taller.

A customised font file is used with battery state and signal timeout chars
Tested on Arduino nano and Auduino IDE 1.8.13