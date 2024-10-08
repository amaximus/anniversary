[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/hacs/integration)

<p><a href="https://www.buymeacoffee.com/6rF5cQl" rel="nofollow" target="_blank"><img src="https://camo.githubusercontent.com/c070316e7fb193354999ef4c93df4bd8e21522fa/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76312e7376673f6c6162656c3d4275792532306d6525323061253230636f66666565266d6573736167653d25463025394625413525413826636f6c6f723d626c61636b266c6f676f3d6275792532306d6525323061253230636f66666565266c6f676f436f6c6f723d7768697465266c6162656c436f6c6f723d366634653337" alt="Buy me a coffee" data-canonical-src="https://img.shields.io/static/v1.svg?label=Buy%20me%20a%20coffee&amp;message=%F0%9F%A5%A8&amp;color=black&amp;logo=buy%20me%20a%20coffee&amp;logoColor=white&amp;labelColor=b0c4de" style="max-width:100%;"></a></p>

# Anniversary sensor for upcoming events for Home Assistant

The state of the sensor will be the number of days till the first upcoming event from the sensor's defined list.
Other related information (sorted list of all events, closest event's icon, event name and anniversary, etc.) will be added as attributes.

#### Installation
The easiest way to install it is through [HACS (Home Assistant Community Store)](https://github.com/hacs/integration),
search for <i>Anniversary</i> in the Integrations.<br />

#### Configuration:
Define sensor with the following configuration parameters:<br />

---
| Name | Optional | `Default` | Description |
| :---- | :---- | :------- | :----------- |
| anniversaries | **Y** | `` | list of events (see below) |
| date_format | **Y** | `%Y-%m-%d` | date format as per [strftime](https://strftime.org).\n %c and %x formats are not supported. |
| items | **Y** | `0` | number of upcoming events to add to the events list attribute |
| multiple | **Y** | `false` | add multiple events when on same date - obsoleted. It will get removed in the next release |
| name | **Y** | `events` | name of the sensor |
| unit_of_measurement | **Y** | `` | custom text, usually days. You may express it in the language of your choice. |
---

Date formats of the defined events have to match date_format parameter, otherwise the event will be ignored and a warning message will be logged.<br>
Since the events are considered yearly repeating events, it does not make sense adding format strings like weekday (%A), time, etc. The date_format parameter refers entirely to this custom integration, do not overcomplicate things, keep it simple. All date_format related exceptions will make that event to get ignored.

Configuration parameters for the list of events:
---
| Name | Optional | `Default` | Description |
| :---- | :---- | :------- | :----------- |
| event | **Y** | `` | name of the event |
| date | **Y** | `` | date of the event. It can contain year information in which case the anniversary attribute will be the number of years passed till the next occurence. |
| icon | **Y** | `mdi:calendar` | icon of the event |
| type | **Y** | `event` | your text choice to denote the event type. Might be useful for templating. |
---

The sensor will set attributes like:
![Anniversary attributes](https://raw.githubusercontent.com/amaximus/anniversary/main/anniversary3.png)

## Example
```
platform: anniversary
name: events
multiple: true
anniversaries:
  - event: 'Doug birthday'
    date: '2000-1-15'
  - event: 'Steve Butabi'
    date: '2000-09-15'
    icon: mdi:cake-variant
  - event: 'Chazz birthday'
    date: '9-15'
    icon: mdi:cake-variant
  - event: 'Frank the tank'
    date: '1998-8-18'
```

#### Lovelace UI
You may use [Custom button card](https://github.com/custom-cards/button-card) to display information on the upcoming event.

You may also combine it with conditional card.

Here is an example using custom button card:
```
type: custom:button-card
size: 30px
styles:
  label:
    - font-size: 90%
  card:
    - height: 80px
  icon:
    - color: var(--paper-item-icon-color)
  grid:
    - position: relative
  custom_fields:
    notification:
      - background-color: >
          [[[
            if (states['sensor.events'].state > 1)
              return "var(--light-primary-color)";
            return "var(--accent-color)";
          ]]]
      - border-radius: 50%
      - position: absolute
      - left: 62%
      - top: 10%
      - height: 20px
      - width: 20px
      - font-size: 9px
      - line-height: 20px
custom_fields:
  notification: >
    [[[ return states['sensor.events'].state ]]]
label: >
  [[[
    var label = states['sensor.events'].attributes.first_event_name
    return label.replace("|","<br>");
  ]]]
color_type: icon
show_label: true
show_name: false
entity: sensor.events
```

Event due in 4 days using default icon:<br>
![Event due in 4 days using default icon](https://raw.githubusercontent.com/amaximus/anniversary/main/anniversary1.png)

Multiple events due tomorrow using custom icon:<br>
![Multiple events due tomorrow using custom icon](https://raw.githubusercontent.com/amaximus/anniversary/main/anniversary2.png)

## Thanks

Thanks to all the people who have contributed!

[![contributors](https://contributors-img.web.app/image?repo=amaximus/anniversary)](https://github.com/amaximus/anniversary/graphs/contributors)
