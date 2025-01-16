# esphome-dual-zone-heating-controller

This is a project for a heating controller based on ESPHome and an ESP32 control board with four relays. 

![20250109_152603-cropped](https://github.com/user-attachments/assets/7383f497-5f0b-4594-a1be-1189d921672c)

*Layout of the controller in an electrical box showing the OLED and back-lit buttons. The top red button is for hot water, middle is for upstairs and bottom is for downstairs.*

# Objectives
The primary objective was to create a controller designed for an S-plan dual-zone and domestic hot-water (DHW) system with individual relays to control each of the zones and hot-water (with one spare relay). (See the link to an S-plan explainer in the Additional Resources below).

A secondary objective was to make the controller so that it is a stand-alone device and not be fully dependent on Home Assistant; it needs to be able to operate the boost functions and the automatic mode in the absence of WiFi or a Home Assistant server. It also needs basic local scheduling of heating times as well as support for domestic hot-water (indirect cylinder) and the two zones (upstairs and downstairs).

# Features:
 - OLED display
 - Three zone multifunction buttons with LED status
 - Multi-function buttons (single press, double press and long press)
 - Boost functions available via buttons or over REST or exposed to HA
 - Internal schedules
 - RTC clock and battery (to support internal schedules)
 - MQTT and API communication to HA (MQTT for possible future leveraging of QoS)
 - Optional internal climate/thermostats (auto-mode) to manage each zone. (Auto mode is dependant on HA for temperature data)
 - Zone or Global disablement from REST/HA

The backlit buttons used in this project are multifunctional: one for boosting the hot water, one for the upstairs zone heating and one for the downstairs zone heating. 
For each of the heating zones there are three functions available:
  - Single press calls a 1hr heating boost
  - Double press (within 5 sec of the first press) calls a 2hr heating boost
  - Long press (hold >3 sec) is a cancellation of boost

The state of each zone is indicated by the LED state of each button:
  - LED off: Zone off
  - Solid on: In boost mode
  - Fading on and off over 1 second: Auto mode

# The 4-way Relay Board:
The board used in this project is the Shenzhen LC "AC/DC Power Supply WiFi Bluetooth BLE 4-way Relay Module". 
http://www.chinalctech.com/cpzx/Programmer/Relay_Module/519.html
It's supplied with solderable header pins for both IO and flashing, so be aware of the need to take out your soldering iron to complete this build. 
It can be powered off AC 220v, DC 7v-30v or 5v DC which makes bench-testing this safe when run at DC voltages. Once installed beside the boiler the enclosure will be powered off a 5A fused spur, so I don't see a need for an additional fuse within the enclosure. 

The IO port mapping of the board is poorly documented but ESPHome does have a brief page on the pinouts and the second site has them well labelled.
https://devices.esphome.io/devices/AC-DC-ESP32-Relay-x4
https://werner.rothschopf.net/microcontroller/202208_esp32_relay_x8_en.htm

![Heating_Controller_labelled](https://github.com/user-attachments/assets/35c3ca37-1f9e-4d52-b6dd-7448ca5dcd5b)

# Controls and Feedback Exposed to Home Assistant:
The controller exposes commonly used functions such as boost options (30 min, 1 hour and 2 hour) for the upstairs and downstairs zones as well as a boost function for the hot water. 

![HA-screen1](https://github.com/user-attachments/assets/5cb6633a-0480-4367-ad9f-962a77ada3b8)

It also provides feedback on the states of each zone, whether that zone is in BOOST mode, TIMER mode, or AUTO mode. For both BOOST and TIMER mode we also get "on until" feedback on the completion time of each action. 

![HA-screen2](https://github.com/user-attachments/assets/ca0305a7-b6b1-44fe-8c40-a8bba1dfc599)

# Components and Circuit Diagram
![Heating_Controller_circuit_v1](https://github.com/user-attachments/assets/a485f4a2-36a9-475e-9d2a-33aabe2441d9)

The Hot Water zone button's output connects to G15 and the LED connects to G4
The Upstairs zone button's output connects to G17 and the LED connects to G18
The Downstairs zone button's output connects to G13 and the LED connects to G14
The ground pins for all three buttons and LED's route to a common connector (Wago in this case) and then back to a GND point on the board
The I2C bus SDA comes from G21 and SDL comes from G22 and loops out from the RTC device to the OLED display
The RTC and OLED both operate off 3.3v so route to 3V3 and GND respectively

# ToDo
 - The switch buttons are assembled using push-fit (or Dupont) jumper wires and these aren't sufficient for long-term use as they tend to loose their retaining pressure over time. I had planned on using IDC headers (known to many of us as IDE hard-drive connectors) in their place as the solder hole pitch on the circuitboard is 2.54mm, but the IDC headers are too long and interfere with surrounding components. So that's on the to-do list for further investigation.
 - I'll upload a circuit diagram shortly, just working out how to best represent it on GitHub.

# Additional Resources:
S-plan wiring explainer courtesy of John Ward: https://flameport.com/electric/central_heating/heating_wiring_S_plan.cs4
