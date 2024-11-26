# esphome-dual-zone-heating-controller

First github project, so bear with me.

This is the YAML for a heating controller based on ESPHome and an ESP32 control board with four relays. 
It's designed for an S-plan dual zone and domestic hot water ("dhw") system with individual relays to control the zone relays, and one spare.

S-plan wiring explainer courtesy of John Ward:
https://flameport.com/electric/central_heating/heating_wiring_S_plan.cs4

It's designed to be a stand-alone system and not dependant on Home Assistant - it includes the following features:

 - RTC clock and battery
 - LCD display
 - Three zone multifunction buttons with LED status
 - Internal schedules
 - Boost functions over REST as well as exposed to HA
 - MQTT communication to HA
