# EaseeSolarCharge
A Home Assistant and node-red based flow to dynamically control Easee EV charging based on Solar Production

![Visualisation of the flow](https://i.imgur.com/c6F0uFs.png)


## Description
This node-red flow allong with a few requesites outlined below will allow you to control the charge current on your Easee EV Charger based on current solar production.
The flow supports 2 or 3 phased charging, where the two-phased is maximizing the charge current, assuming that you have an summarazing electricity-meter, where you can export on one phase and import on the other two phases equalling each other.

## Prerequesites
- The flow is intended for European grid with 3-phase 230v/400v installations and 3-phase Easee EV charger
- You need a solar-array, preferably with a 3 phase inverter, and you need live-data for the current production in Home Assistant (preferably data updating every 5-30 seconds)
- You need a Home Assistant, with the [Easee Integration](https://github.com/fondberg/easee_hass) installed.
- You need to create two Boolean helpers in Home Assistant (one for activation of SolarCharge, and one for switching between 2 or 3 phased cars)
- You need to create a sensor for creating 2minute average production values: 
 ```
   - platform: filter
    name: Growatt 5min Average Production
    entity_id: sensor.growatt_grid_active_power
    filters:
      - filter: time_simple_moving_average
        window_size: "00:05"
 ```
