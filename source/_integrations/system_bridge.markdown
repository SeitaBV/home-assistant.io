---
title: System Bridge
description: How to integrate the System Bridge integration into Home Assistant.
ha_category:
  - Sensor
  - System Monitor
ha_release: 2021.6
ha_iot_class: Local Push
ha_config_flow: true
ha_codeowners:
  - '@timmo001'
ha_domain: system_bridge
ha_quality_scale: silver
ha_platforms:
  - binary_sensor
  - sensor
ha_zeroconf: true
ha_integration_type: integration
---
[System Bridge](https://system-bridge.timmo.dev) is an application that runs on your local machine to share system information via its API/WebSocket. You can also send commands to the device such as opening a URL or sending keyboard keypresses.
## Prerequisites
### Version
This integration requires System Bridge 3.1.1 and above. Any older version will not work.
### API Key
You will need your API key. This can be found following the documentation [here](https://system-bridge.timmo.dev/docs/running).
{% include integrations/config_flow.md %}
## Binary Sensors
This integration provides the following binary sensors:
| Name                  | Description                        |
| --------------------- | ---------------------------------- |
| Battery Is Charging   | Whether the battery is charging    |
| New Version Available | Whether a new version is available |
## Sensors

This integration provides the following sensors:

| Name                 | Description                                         |
| -------------------- | --------------------------------------------------- |
| Battery              | Battery level of the device                         |
| Boot Time            | Time the device was turned on                       |
| CPU Speed            | The current CPU speed                               |
| Displays Connected   | Number of displays connected                        |
| Display Resolution X | Display resolution (across)                         |
| Display Resolution Y | Display resolution (down)                           |
| Display Refresh Rate | Display refresh rate                                |
| Filesystem(s)        | Space used for each drive letter / filesystem mount |
| GPU Memory Free      | GPU memory free in GB                               |
| GPU Usage %          | GPU usage percentage                                |
| Kernel               | Version information of the Kernel                   |
| Latest Version       | System Bridge Latest Version                        |
| Load                 | System load percentage                              |
| Memory Free          | Memory (RAM) free in GB                             |
| Memory Used          | Memory (RAM) used in GB                             |
| Memory Used %        | Memory (RAM) % used                                 |
| Operating System     | Version information of the Operating System         |
| Version              | System Bridge Version                               |

These sensors are also available, but are not enabled by default:

| Name                   | Description                        |
| ---------------------- | ---------------------------------- |
| CPU Temperature        | The current temperature of the CPU |
| CPU Voltage            | The current voltage of the CPU     |
| GPU Core Clock Speed   | GPU core clock speed in MHz        |
| GPU Memory Clock Speed | GPU memory clock speed in MHz      |
| GPU Fan Speed          | GPU fan speed percentage           |
| GPU Memory Used        | GPU memory used in GB              |
| GPU Memory Used %      | GPU memory used percentage         |
| GPU Power Usage        | GPU power usage                    |
| GPU Temperature        | The current temperature of the GPU |

## Media Source

This integration is available as a media source to use with the media browser integration. You can browse and view media from your system to media players such as your web browser and other supported media players.

## Services

### Notifications `notify.system_bridge_hostname`

You can send notifications to the device using the `notify` platform.

```yaml
service: notify.system_bridge_hostname
data:
  data:
    image: "https://brands.home-assistant.io/system_bridge/logo@2x.png"
    timeout: 30
    actions:
      - command: api
        data:
          endpoint: open
          method: POST
          body:
            url: "http://homeassistant.local:8123/lovelace/cameras"
        label: "Open Cameras"
    audio:
      source: "https://d3qhmae9zx9eb.cloudfront.net/home/amzn_sfx_doorbell_chime_02.mp3"
      volume: 80
  title: "Test Title"
  message: "This is a message"
```

#### Parameters

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| target    | The target to send the notification to. This can be ignored. |
| title     | The title of the notification.                               |
| message   | The message of the notification.                             |
| data      | The data to send to the device. See below for info.          |

##### Actions (`data` parameter)

This is an array of actions that can be sent to the device. These are buttons that show below the title, message and image.

| Parameter | Description                                                                                                                |
| --------- | -------------------------------------------------------------------------------------------------------------------------- |
| command   | The command to send to the device. For example `api` will send a request to the System Bridge API.                         |
| label     | The label of the button.                                                                                                   |
| data      | The data to send to the device. The available parameters for the `api` command are: `endpoint`, `method` `body`, `params`. |

Here is an example action that will open a URL in the device's browser:

```yaml
- command: api
  label: "Open Cameras"
  data:
    endpoint: open
    method: POST
    body:
      url: "http://homeassistant.local:8123/lovelace/cameras"
```

##### Audio (`data` parameter)

This is an object containing the `source` and `volume` (0-100). The source must be a URL to a playable audio file (an MP3 for example).

### Service `system_bridge.open_path`

Open a URL or file on the server using the default application.
{% my developer_call_service service="system_bridge.open_path" title="Show service in your Home Assistant instance." %}
```yaml
service: system_bridge.open_path
data:
  bridge: "deviceid"
  path: "C:\\image.jpg"
```
### Service `system_bridge.open_url`
Open a URL or file on the server using the default application.
{% my developer_call_service service="system_bridge.open_url" title="Show service in your Home Assistant instance." %}
```yaml
service: system_bridge.open_url
data:
  bridge: "deviceid"
  url: "https://home-assistant.io"
```
### Service `system_bridge.send_keypress`
Send a keypress to the server.
{% my developer_call_service service="system_bridge.send_keypress" title="Show service in your Home Assistant instance." %}
```yaml
service: system_bridge.send_keypress
data:
  bridge: "deviceid"
  key: "a"
```
### Service `system_bridge.send_text`
Sends text for the server to type.
{% my developer_call_service service="system_bridge.send_text" title="Show service in your Home Assistant instance." %}
```yaml
service: system_bridge.send_text
data:
  bridge: "deviceid"
  text: "Hello"
```

### Service `system_bridge.power_command`

Sends power command to the system.

{% my developer_call_service service="system_bridge.power_command" title="Show service in your Home Assistant instance." %}


```yaml
service: system_bridge.power_command
data:
  bridge: "device"
  command: "sleep"
```

Supported commands are:

- `hibernate`
- `lock`
- `logout`
- `restart`
- `shutdown`
- `sleep`
