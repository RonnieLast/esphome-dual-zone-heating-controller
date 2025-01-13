# esphome-dual-zone-heating-controller

This is the YAML for a heating controller based on ESPHome and an ESP32 control board with four relays. 

![20250109_152603-cropped](https://github.com/user-attachments/assets/7383f497-5f0b-4594-a1be-1189d921672c)

Above: Layout of the controller in an electrical box showing the OLED and back-lit buttons. The top red button is for hot water, middle is for upstairs and bottom is for downstairs.

It's designed as a controller for an S-plan dual zone and domestic hot water system with individual relays to control the zone relays, and one spare relay.
See the link to an S-plan explainer below.

A primary objective is to make the controller stand-alone and not be fully dependent on Home Assistant; it needs to be able to operate the boost functions and the automatic mode in the absence of WiFi or a Home Assistant server. It also needs basic local scheduling of heating times as well as support for domestic hot-water (indirect cylinder) and the two zones (upstairs and downstairs).

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

The backlit buttons are multifunctional: one for boosting the hot water, one for the upstairs zone heating and one for the downstairs zone heating. 
For each of the heating zones there are three functions available:
  - Single press calls a 1hr heating boost
  - Double press (within 5 sec of the first press) calls a 2hr heating boost
  - Long press (hold >3 sec) is a cancellation of boost

The state of each zone is indicated by the LED state of each button:
  - LED off: Zone off
  - Solid on: In boost mode
  - Fading on and off over 1 second: Auto mode

The 4-way Relay Board:
The board used in this project is the Shenzhen LC "AC/DC Power Supply WiFi Bluetooth BLE 4-way Relay Module". 
http://www.chinalctech.com/cpzx/Programmer/Relay_Module/519.html
It's supplied with solderable header pins for both IO and flashing, so be aware of the need to take out your soldering iron to complete this build. 
It can be powered off AC 220v, DC 7v-30v or 5v DC which makes bench-testing this safe when run at DC voltages. Once installed beside the boiler the enclosure will be powered off a 5A fused spur, so I don't see a need for an additional fuse within the enclosure. 

The IO port mapping of the board is poorly documented but ESPHome does have a brief page on the pinouts and the second site has them well labeled.
https://devices.esphome.io/devices/AC-DC-ESP32-Relay-x4
https://werner.rothschopf.net/microcontroller/202208_esp32_relay_x8_en.htm

![circuit_image](https://github.com/user-attachments/assets/1e9925cb-4f6b-49eb-8737-a9f256f0795a)


Controls Exposed and Feedback to Home Assistant:
The controller exposes commonly used functions such as boost opions (30 min, 1 hour and 2 hour) for the upstairs and downstairs zones as well as a boost function for the hot water. 

![HA-screen1](https://github.com/user-attachments/assets/5cb6633a-0480-4367-ad9f-962a77ada3b8)

It also provides feedback on the states of each zone, whether that zone is in BOOST mode, TIMER moder, or AUTO mode. For both BOOST and TIMER mode we also get "on until" feedback on the completion time of each action. 

![HA-screen2](https://github.com/user-attachments/assets/ca0305a7-b6b1-44fe-8c40-a8bba1dfc599)

Additional Resources:
S-plan wiring explainer courtesy of John Ward: https://flameport.com/electric/central_heating/heating_wiring_S_plan.cs4
