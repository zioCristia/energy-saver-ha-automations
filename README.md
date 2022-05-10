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
* [Automatic ac power on](###automatic-ac-power-on)
* [Automatic ac power on](###automatic-ac-power-off)

Please see the file [airConditionersAutomations.yaml.example](/airConditionersAutomations.yaml.example) for the complete yaml.

### Ac power on
This automation is used to turn on all the ac units with the same setted temperature and setted mode. This is done by taking into consideration the option choosen in an input select for the ac mode, and an input number for set temperature. The trigger of the automation is an input boolean, specifically when it switches from off to on. In this way I can either turn on all by clicking in a button or let an automation do it.

Here is the complete YAML code:
```
- id: '1647087606051'
  alias: Ac power on
  description: 'We power on all the ac units based on the two input_select, 
    one for the temperature and the other for the mode'
  trigger:
  - platform: state
    entity_id: input_boolean.on_off_ac
    from: 'off'
    to: 'on'
  condition: []
  action:
  - service: climate.set_hvac_mode
    entity_id: climate.panasonic_sala
    data:
      hvac_mode: "{% if is_state(\"input_select.modalita\", \"caldo\") %}\n  heat\n\
        {% elif is_state(\"input_select.modalita\", \"freddo\") %}\n  cool\n{% elif\
        \ is_state(\"input_select.modalita\", \"secco\") %}\n  dry\n{% elif is_state(\"\
        input_select.modalita\", \"automatico\") %}\n  heat_cool\n{% elif is_state(\"\
        input_select.modalita\", \"solo ventilatore\") %}\n  fan_only\n{% endif %}\n"
  - service: climate.set_hvac_mode
    entity_id: climate.panasonic_camera
    data:
      hvac_mode: "{% if is_state(\"input_select.modalita\", \"caldo\") %}\n  heat\n\
        {% elif is_state(\"input_select.modalita\", \"freddo\") %}\n  cool\n{% elif\
        \ is_state(\"input_select.modalita\", \"secco\") %}\n  dry\n{% elif is_state(\"\
        input_select.modalita\", \"automatico\") %}\n  heat_cool\n{% elif is_state(\"\
        input_select.modalita\", \"solo ventilatore\") %}\n  fan_only\n{% endif %}\n"
  - service: climate.set_hvac_mode
    entity_id: climate.panasonic_camera_2
    data:
      hvac_mode: "{% if is_state(\"input_select.modalita\", \"caldo\") %}\n  heat\n\
        {% elif is_state(\"input_select.modalita\", \"freddo\") %}\n  cool\n{% elif\
        \ is_state(\"input_select.modalita\", \"secco\") %}\n  dry\n{% elif is_state(\"\
        input_select.modalita\", \"automatico\") %}\n  heat_cool\n{% elif is_state(\"\
        input_select.modalita\", \"solo ventilatore\") %}\n  fan_only\n{% endif %}\n"
  - service: climate.set_temperature
    target:
      entity_id: climate.panasonic_sala
    data:
      temperature: '{{states(''input_number.temperatura_condizionatori'')}}'
  - service: climate.set_temperature
    target:
      entity_id: climate.panasonic_camera
    data:
      temperature: '{{states(''input_number.temperatura_condizionatori'')}}'
  - service: climate.set_temperature
    target:
      entity_id: climate.panasonic_camera_2
    data:
      temperature: '{{states(''input_number.temperatura_condizionatori'')}}'
  mode: single
```

### Ac power off

### Automatic ac power on

### Automatic ac power off

# Water heater automations
