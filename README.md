# esphome-dual-zone-heating-controller

This is the YAML for a heating controller based on ESPHome and an ESP32 control board with four relays. 

![20241128_102516-rotated-noexif](https://github.com/user-attachments/assets/7201aabb-6e90-4321-95c5-0a7c633bf3b6)
Above: Rough layout of the relay board in an electrical box showing the OLED and back-lit buttons.

It's designed as a controller for an S-plan dual zone and domestic hot water ("DHW") system with individual relays to control the zone relays, and one spare relay.
See the link to an S-plan explainer below.

A primary objective is to make the controller a stand-alone system and not be fully dependant on Home Assistant; I want to be able to operate the boost functions and the auto-mode in the absence of WiFi or a Home Assistant server. I also want some basic local scheduling of heating times as well as support for domestic hot-water (indirect cylinder) and the two zones (upstairs and downstairs).

Features:
 - OLED display
 - Three zone multifunction buttons with LED status
 - Internal schedules
 - RTC clock and battery (to support internal schedules)
 - Boost function available via buttons, over REST as well as exposed to HA
 - Multi-function buttons (single press, double press and long press)
 - MQTT and API communication to HA
 - Optional internal climate/thermostats (auto-mode) to manage each zone. (Auto mode is dependant on HA for temperature data)
 - Zone or Global disablement from REST/HA

The backlit buttons are multifunctional: one for boosting DHW, one for the upstairs zone heating and one for the downstairs zone heating. 
For each of the heating zones there are three functions available:
  - Single press calls a 1hr heating boost
  - Double press (within 5 sec of the first press) calls a 2hr heating boost
  - Long press (hold >3 sec) is a cancellation of boost

The state of each zone is indicated by the LED state of each button:
  - Button LED off: Zone off
  - On solid: In boost mode
  - Fading on and off over 1 second: Auto mode

The 4-way Relay Board:
The board used in this project is the Shenzhen LC "AC/DC Power Supply WiFi Bluetooth BLE 4-way Relay Module". 
http://www.chinalctech.com/cpzx/Programmer/Relay_Module/519.html
It's supplied with solderable header pins for both IO and flashing, so be aware of the need to take out your soldering iron to complete this build. 
It can be powered off AC 220v, DC 7v-30v or 5v DC which makes bench-testing this safe when run at DC voltages. Once installed beside the boiler the enclosure will be powered off a 5A fused spur, so I don't see a need for an additional fuse within the enclosure. 

The IO port mapping of the board is poorly documented but ESPHome does have a brief page on the pinouts and the second site has them well labeled.
https://devices.esphome.io/devices/AC-DC-ESP32-Relay-x4
https://werner.rothschopf.net/microcontroller/202208_esp32_relay_x8_en.htm


Additional Resources:
S-plan wiring explainer courtesy of John Ward: https://flameport.com/electric/central_heating/heating_wiring_S_plan.cs4
