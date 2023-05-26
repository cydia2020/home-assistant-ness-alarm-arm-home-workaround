## What this does

There is a known bug in the Ness Alarm (ness_alarm) component in Home Assistant that prevents the system from distinguishing between armed home and armed away (or armed home from disarmed) in certain situations. This repository contains the necessary steps involved in working around the bug. It requires an AUX output board (p.n.: ness 106-013, available at RadioParts) and an ESP8266.

## My setup

<img src="/my_setup.jpg"  width=50% height=50%>

My setup is a bit more janky and complicated as I am relaying Blue Iris's alerts to the alarm system as well. The minimal setup for this involves only 3 pairs of wires connected to the alarm panel, and everything should fit inside the alarm's housing.

## Wiring

1. Follow the alarm's instructions and connect the aux relay board to the aux header on the alarm system's motherboard.
2. Choose 2 GPIO pins on your ESP8266, and connect a 10k resistor between 3v3 and each of those pins (PULLUP mode).
3. Connect the COMMON pin of the relay to the ESP8266's ground, and connect the N.O. (normally open) PIN to your desired GPIO on the ESP8266.
4. Using a buck converter, connect the ESP8266 to your alarm panel's 12V DC output.

## Alarm Settings

For this section, you will need your alarm panel's master code and installer code. If you do not have these, follow the "How to enter Program Mode" section on page 13 of the alarm's manual.

To enter installer program mode with both codes, use "P**masterCode**E", followed by "P**installerCode**E".

For example: If your master code is 1234, and your installer code is 987654, you will enter "P1234E", then followed by "P987654E" to access installer program mode.

Once you are in the Installer Program Mode:
1. Press P142E on the alarm's keypad, then followed by 1E (this will trigger the alarm's AUX2 output when the alarm's zone 1 is armed).
2. Press P143E on the alarm's keypad, then followed by 3E (this will trigger the alarm's AUX3 output when the alarm's home mode is armed).

Feel free to adjust these based on your needs. You might want to use AUX1/AUX4 as your output, or you might have a zone 2.

For more details, check out pages 58-66 in the manual.

## ESP8266 Programming

I used ESPHome as it's easy to set up and update. Feel free to use whatever suits you. I've included my ESPHome configuration in this repository for your reference.

## Home Assistant Template Alarm Configuration

A template alarm works great in this situation; however, you do lose the ability to disarm with your code in the Home Assistant frontend. It shouldn't matter too much in this case nonetheless. My configuration has also been included in this repository for your reference.
