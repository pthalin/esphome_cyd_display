# Esphome CYD Display
In the file [display1.yaml](display1.yaml) is the Esphome yaml code for a display I use with Home Assistant. It displays outdoor temperature with a graph and by touching the display is will show electricty price and consumtion for 5 seconds.

<img height="300" src="images/temperature_graph.jpg"><img height="300" src="images/electricity.jpg">

A new sensor that collects history data as an attribute is need in Home Assistan for the temerature history graph. Replace YOUR_SENSOR with you sensor name. After adding this to your configuration.yaml you need to wait 24h for the data to collected for the fist time.

```
template:
  - trigger:
      - trigger: time_pattern
        minutes: "/5"
    sensor:
      - name: "Outdoor Temperature Logger 24h"
        unique_id: outdoor_temperature_logger_24h
        state: "{{ states('sensor.YOUR_SENSOR') }}"
        unit_of_measurement: "°C"
        attributes:
          history: >
            {% set current_temp = states('sensor.YOUR_SENSOR') | float(0) %}
            {% set list = state_attr('sensor.outdoor_temperature_logger_24h', 'history') or [] %}

            {# Append new value #}
            {% set updated_list = list + [current_temp] %}

            {# Limit to last 24 hours (288 intervals of 5 minutes) #}
            {{ updated_list[-288:] }}
```

### Parts (AliExpress)
Use this arfilliate link to support me!\
[ESP32-2432S028R aka ESP CYD ](https://s.click.aliexpress.com/e/_c3LTtMYf)


### 3D Printed Case for CYD
Files:\
[Top](./case/CYD_Case_Top.stl)\
[Bottom](./case/CYD_Case_Bottom.stl)

If you have a printer without a heated bed it will likely wrap and the corners will lift and detach from the bed. This can be prevent by adding a Brim.\
<img height="300" src="images/3d_print_brim.png"> 


Donation using [Ko-Fi](https://ko-fi.com/patriksretrotech) or [PayPal](https://www.paypal.com/donate/?business=UCTJFD6L7UYFL&no_recurring=0&item_name=Please+support+me%21&currency_code=SEK) are highly appreciated!
