# EaseeSolarCharge
A Home Assistant and Node-RED based flow to dynamically control Easee EV charging based on Solar Production

![Visualisation of the flow](https://i.imgur.com/c6F0uFs.png)


## Description
This node-red flow allong with a few requesites outlined below will allow you to control the charge current on your Easee EV Charger based on current solar production.
The flow supports 2 or 3 phased charging, where the two-phased is maximizing the charge current, assuming that you have an summarazing electricity-meter, where you can export on one phase and import on the other two phases equalling each other.

## Prerequisites
- The flow is intended for European grid with 3-phase 230v/400v installations and 3-phase Easee EV charger
- You need a solar-array, preferably with a 3 phase inverter, and you need live-data for the solar production (in watts) in Home Assistant (preferably data updating every 5-30 seconds) The flow is designed for a solar array with a max capacity of 7500w, feel free to extend the switch-nodes to higher wattage if your array is larger, or reduce the number of switch points if your array is smaller.
- You need a Home Assistant, with the [Easee Integration](https://github.com/fondberg/easee_hass) installed.
- You need to create two Boolean helpers in Home Assistant (one for activation of SolarCharge, and one for switching between 2 or 3 phased cars):
  - input_boolean.solarcharging (Put this helper in a Lovelace dashboard to be able to switch the flow on or off)
  - input_boolean.3phasecharging (Put this helper in a Lovelace dashboard to be able to switch between 3 and 2 phase charging (on=3-phase, off=2-phase))
- You need to create a sensor for creating 2minute average production values (correct code to your own scenario): 
 ```
sensor: 
  - platform: filter
    name: Inverter 2min Average Production
    entity_id: sensor.inverter_grid_active_power
    filters:
      - filter: time_simple_moving_average
        window_size: "00:02"
 ```
- Feel free to delete the bottom part of the flow that auto enable/disable Solar Charging at sunset/sunrise. That does not impact the function of the main flow.

## Installation
- Check off the boxes in the Prerequisites chapter
- Grab the .json file in this repository and import it to Node-RED
- Adjust nodes to your entities etc.
- Be sure to enter your Easee Charger ID the the 3 Charger ID Change Nodes:
![Charger ID Settings](https://i.imgur.com/NVquk1Y.png)
