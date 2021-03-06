# Energy saver Home Assistant automations
Home assistant automations created to maximize the use of solar power in my home to control the air conditioners and the water heater. 
Any possibility of improvement is accepted.

## Table of contents
* [General info](#general-info)
* [Air conditioners automations](#air-conditioners-automations)
  * [Ac power off](#ac-power-off)
  * [Ac power on](#ac-power-on)
  * [Automatic ac power on](#automatic-ac-power-on)
  * [Automatic ac power off](#automatic-ac-power-off)  
* [Water heater automations](#water-heater-automations)
  * [Wh power on](#wh-power-on)
  * [Wh power off](#wh-power-off)
  * [Wh power off overpower](#wh-power-off-overpower)

# General info
These automations are made possible thanks to the other EspHome devices I created ([esp-energy-monitor](https://github.com/zioCristia/esp-energy-monitor) and [esp-smart-water-heater](https://github.com/zioCristia/esp-smart-water-heater)) and the [DomyStyle](https://github.com/DomiStyle) guide [esphome-panasonic-ac](https://github.com/DomiStyle/esphome-panasonic-ac).

The goal is to use as much energy as possible produced by the PV system since I don't have a storage battery for the house.
Therefore I activate (or deactivate) the devices when I produce (not produce) enough energy.

In the [configuration.yaml.example](/configuration.yaml.example) we can see some components which I have added to the configuratin.yaml file of my home assistant to allow the creation of the automations.

# Air conditioners automations
I have 3 different air conditioner units in my home and to control them all at the same time I created 4 different automations for the air conditioners:
* [Ac power off](###ac-power-off)
* [Ac power on](###ac-power-on)
* [Automatic ac power on](###automatic-ac-power-on)
* [Automatic ac power off](###automatic-ac-power-off)

I can either turn the ac units on/off by clicking on a button or letting the automations do that when it is more energetically convenient (thanks to the third and the fourth one). I've also added a button to deactivate the automatic power on/off.

Please see for more information, the file [airConditionersAutomations.yaml.example](/airConditionersAutomations.yaml.example) for the complete yaml or read the following sub-chapters.

![alt text](/images/ac-view.jpg)
![alt text](/images/ac-view-detail.jpg)

### Ac power on
This automation is used to turn on all the ac units with the same set temperature and set mode. This is done by taking into consideration the option chosen in an input select for the ac mode, and an input number for set temperature. The trigger of the automation is an input boolean, specifically when it switches from off to on. In this way I can either turn it on all by clicking on a button or let an automation do it.

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
This automation just shuts off all the three ac units and it's triggered by the same input_boolean as above, when change the state from on to off. As in the previous automation, in this way I can either turn on all by clicking on a button or let an automation do it.

Here is the complete YAML code:
```
- id: '1645792539683'
  alias: Ac power off
  description: ''
  trigger:
  - platform: state
    entity_id: input_boolean.on_off_ac
    from: 'on'
    to: 'off'
  condition: []
  action:
  - device_id: afd9ef7c6f698e86f51ab05662153e9b
    domain: climate
    entity_id: climate.panasonic_sala
    type: set_hvac_mode
    hvac_mode: 'off'
  - device_id: da1cb3ea8655d70135f3a64230e4a129
    domain: climate
    entity_id: climate.panasonic_camera
    type: set_hvac_mode
    hvac_mode: 'off'
  - device_id: ded249cf099e198cdf8627a08c1714a6
    domain: climate
    entity_id: climate.panasonic_camera_2
    type: set_hvac_mode
    hvac_mode: 'off'
  mode: single
```

### Automatic ac power on
This automation allows to automatically turn on the conditioners by switching on the `input_boolean.on_off_ac` which is used as a triggered in the [Ac power on](#ac-power-on) automation. This can be done only when the `input_boolean.ac_automationsc` button is on. 
The trigger is the following custom binary sensor crated (saved in the `configuration.yaml` file as [here](/configuration.yaml.example)):
```
- platform: template
    sensors:
      ac_on_5min:
        value_template: >
          {% set diff = states('sensor.power_difference') | float %}
          {% if diff > 1800 %}
            {{ true }}
          {% else %}
            {{ false }}
          {% endif %}
        delay_on:
          minutes: 5
```
The sensor switches on whenever we have, for more than 5 minutes, an excess of more than 1800W in the house, calculated as (power in from the solar panels) - (power used in the house) as seen [here](https://github.com/zioCristia/esp-energy-monitor#software). This allows them to not continuously switch on/off the conditioners which is not so good for their preservation.

Here is the complete YAML code of the automation:
```
- id: '1647084941079'
  alias: Automatic ac power on
  description: ''
  trigger:
  - platform: state
    entity_id: binary_sensor.ac_on_5min
    from: 'off'
    to: 'on'
  condition:
  - condition: state
    entity_id: input_boolean.ac_automations
    state: 'on'
  action:
  - service: input_boolean.turn_on
    target:
      entity_id: input_boolean.on_off_ac
  mode: single
```

### Automatic ac power off
Finally, this automation turns off automatically the conditioners when we don't produce anymore sufficient power with the solar panels. The trigger element is `binary_sensor.lowcurrent_10min` which switches on when for at least 10 minutes the power difference (in_solar_panels - out_house) is lower than -100W or we don't produce anything from the solar panels.
This is the code I have used:

```
- platform: template
    sensors:
      lowcurrent_10min:
        value_template: >
          {% set diff = states('sensor.power_difference') | float %}
          {% set solar = states('sensor.power_solar') | float %}
          {% if diff <  -100 or solar == 0 %}
            {{ true }}
          {% else %}
            {{ false }}
          {% endif %}
        delay_on:
          minutes: 10
```
And here you can see the automation:
```
- id: '1647085086749'
  alias: Automatic ac power off
  description: ''
  trigger:
  - platform: state
    entity_id: binary_sensor.lowcurrent_10min
    from: 'off'
    to: 'on'
  condition:
  - condition: state
    entity_id: input_boolean.ac_automations
    state: 'on'
  action:
  - service: input_boolean.turn_off
    target:
      entity_id: input_boolean.on_off_ac
  mode: single
```

# Water heater automations
For the water heater I've created 3 different automations, 2 for switching it off and one for turning it on.

* [Wh power on](###wh-power-on)
* [Wh power off](###wh-power-off)
* [Wh power off overpower](###wh-power-off-overpower)

They are activated based on the power consumption of the house and the power production of the house so that maximize the use of free power from them. The water heater has a 1200W resistance which I consider to do the automation and the maximum power consumption in my house is 3000W. Of course, these values are corrected to limit the continuous trigger of the automations.

![alt text](/images/water-heater-view.jpg)

### Wh power on
With this automation I wanted to turn on the boiler automatically when we produce sufficient power from the solar panels and at the same time, I want to be sure that turning it on won't exceed the 3kW max power of my home. That's why I have two triggers that will control simultaneously the two conditions and whenever one is triggered, we must always respect also the other. In fact, they are both also in the condition paragraph.

Moreover, to avoid a continuous power on/off of the water heater, I've added a condition where the wh can switch on only if it was off for at least 1 minute. As we will see there is not something similar in the power off automation because I want the boiler to be the first to be turned off whenever I exceed the 3000W power production in the house.

The following is the code for the `inary_sensor.boil_off_1min` used in the condition:

```
- platform: template
    sensors:
      boil_off_1min:
        value_template: >
          {{ is_state('switch.switch_boiler', 'off')}}
        delay_on:
          minutes: 1
```

The last condition is used to allow to power off all the three automations from the UI of Home Assistant thanks to a button.

Here is the complete automation code:

```
- id: '1640205176027'
  alias: Water heater power on
  description: Automation that allows us to turn on and off the boiler according 
    to the current we produce with the photovoltaic system to which we 
    subtract the one used by the house.
  trigger:
  - platform: numeric_state
    entity_id: sensor.power_difference
    above: '1000'
  - platform: numeric_state
    entity_id: sensor.home_power
    below: '1750'
  condition:
  - condition: numeric_state
    entity_id: sensor.home_power
    below: '1750'
  - condition: state
    entity_id: switch.switch_boiler
    state: 'off'
  - condition: numeric_state
    entity_id: sensor.power_difference
    above: '1000'
  - condition: state
    entity_id: binary_sensor.boil_off_1min
    state: 'on'
  - condition: state
    entity_id: input_boolean.automazioni_boiler
    state: 'on'
  action:
  - type: turn_on
    device_id: 68152b6869280c61c08bc1c6b7c1d404
    entity_id: switch.switch_boiler
    domain: switch
  mode: single
```

### Wh power off
This is the automation that will turn off the water heater when we don't produce enough power from the solar panels any more. We consider always the `sensor.power_difference` to take into account any other consumption in the house such as washing machine, dishwasher, etc...

The trigger action is the `sensor.power_difference` below -250W to consider fluctuations due to both the power of the house and the power of the sun.

The code is the following:

```
- id: '1641921739360'
  alias: Water heater power off
  description: 'We turn off the water heater when the power difference (IN-OUT) is less than -250W'
  trigger:
  - platform: numeric_state
    entity_id: sensor.power_difference
    below: '-250'
  condition:
  - condition: state
    entity_id: switch.switch_boiler
    state: 'on'
  - condition: state
    entity_id: input_boolean.automation_boiler
    state: 'on'
  action:
  - type: turn_off
    device_id: 68152b6869280c61c08bc1c6b7c1d404
    entity_id: switch.switch_boiler
    domain: switch
  mode: single
```

### Wh power off overpower
The last automation is used to turn off the water heater when, although we produce enough power from the solar panels, we exceed the maximum usable power limit of the house. 
This is precisely 3000W, but 3100W was used to have a greater margin to fluctuations.
```
- id: '1640874461166'
  alias: Water heater power off overpower
  description: 'We turn off the water heater when we consume more than 3100W 
    in the house so as to avoid the general power cut.'
  trigger:
  - platform: numeric_state
    entity_id: sensor.home_power
    above: '3100'
  condition:
  - condition: state
    entity_id: switch.switch_boiler
    state: 'on'
  - condition: state
    entity_id: input_boolean.automation_boiler
    state: 'on'
  action:
  - type: turn_off
    device_id: 68152b6869280c61c08bc1c6b7c1d404
    entity_id: switch.interruttore_boiler
    domain: switch
  mode: single
```
