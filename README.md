# Energy saver Home Assistant automations
Home assistant automations created to maximise the use of solar power in my home to control the air conditioners and the water heater.

## Table of contenets
* [General info](#general-info)
* [Air conditioners automations](#air-conditioners-automations)
* [Water heater automations](#water-heater-automations)

# General info
These automations are made possible thanks to the other EspHome devices I created ([esp-energy-monitor](https://github.com/zioCristia/esp-energy-monitor) and [esp-smart-water-heater](https://github.com/zioCristia/esp-smart-water-heater)) and the [DomyStyle](https://github.com/DomiStyle) guide [esphome-panasonic-ac](https://github.com/DomiStyle/esphome-panasonic-ac).

The goal is to use as much energy as possible produced by the PV system since I don't have a storage battery for the house.
Therefore I activate (or deactivate) the devices when I produce (not produce) enough energy.

In the [configuration.yaml.example](/configuration.yaml.example) we can see some components which I have added to the configuratin.yaml file of my home assistant to allows the creation of the automations.

# Air conditioners automations
I have 3 different air conditioner units in my home and to control them all at the same time I created 4 different automations for the air conditioners:
* [Ac power off](###ac-power-off)
* [Ac power on](###ac-power-on)
* [Automatica ac power on](###automatic-ac-power-on)
* [Automatica ac power on](###automatic-ac-power-off)

Please see the file [airConditionersAutomations.yaml.example](/airConditionersAutomations.yaml.example) for the complete yaml.

### Ac power on

### Ac power off

### Automatic ac power on

### Automatic ac power off

# Water heater automations
