

NOTE: Version 2.1 adds support for **Occupancy Sensors &  Weather Sensors** - see more details below
______________
homebridge-tado-ac (v2.1)
========================

Homebridge Plugin for Tado Smart AC Control.

Complies with ```Service.HeaterCooler```

Compatible with ***iOS 11 and above*** -  iOS 10 Home app does not support this service.

### Whats New in Version 2 ??

This version is a whole new build that brings a lot more options to control your air conditiner with Homekit:

1. Homebridge plugin turned into platform to support 1 eco-system.
2. Device auto detection - the plugin automatically detects and adds any Tado AC device.
3. HomeKit air conditioner support.
4. Support for ***Swing*** and ***Rotation Speed*** of the air conditioner
5. Fan Support - as different accessory.
6. Auto configuration - The plugin detects the capabilities of your AC (set by Tado) and set them up accordingly in Homekit:
    - Zone Id
    - Home Id
    - Min/Max Temperature
    - Modes (AUTO/COOL/HEAT/FAN)
    - Temperature Unit
    - Swing
    - Rotation Speeds
7. Humidity sensor as a seperated accessory
8. Much faster status update - makes the accessories load much faster in Home app
9. Easy config - Only username and password are required now to enjoy all features.

**from version 2.1:**

10. Outside Temperature support
11. Solar Intensity support
12. Occupancy Sensors.


This version does not support lower version then iOS 11,
To use old version feel free to install from [GitHub](https://github.com/nitaybz/homebridge-tado-ac-old):
`sudo npm install -g https://github.com/nitaybz/homebridge-tado-ac-old.git`

## Installation

1. Install homebridge using: `sudo npm install -g homebridge`
2. Install this plugin using: `sudo npm install -g homebridge-tado-ac`
3. Update your configuration file. See `sample-config.json` in this repository for a sample.

**install from git (latest version): `sudo npm install -g https://github.com/nitaybz/homebridge-tado-ac.git`

## Config file

#### Easy config:
```
"platforms": [
    {
        "platform": "TadoAC",
        "username": "user@name.com",
        "password": "*************"
    }
]
```

#### Advanced config:
```
"platforms": [
    {
        "platform": "TadoAC",
        "name": "Tado AC",
        "username": "user@name.com",
        "password": "*************",
        "tadoMode": "MANUAL",
        "weatherSensorsEnabled": true,
        "weatherPollingInterval": 10,
        "occupancySensorsEnabled": true,
        "occupancyPollingInterval": 10,
        "anyoneSensor": true
    }
]
```

## Configurations

|             Parameter            |                       Description                       | Required |  Default  |
| -------------------------------- | ------------------------------------------------------- |:--------:|:---------:|
| `platform`                       | always "TadoAC"                                         |     ✓    |      -    |
| `name`                           | name of the platform - for logs only                    |          |      -    |
| `username`                       | your tado account username (something@something.com)    |     ✓    |      -    |
| `password`                       | your tado account password                              |     ✓    |      -    |
| `tadoMode`                       | default mode for the commands to be sent with. can be "MANUAL" for manual control until ended by the user, or "TADO_MODE" for manual control until next schedule change in tado app .          |             |  "MANUAL" |
| `weatherSensorsEnabled`          | Enable **Outside Temperature** sensor and **Solar Intensity** light bulb.  **more details below*      |             |  false |
| `weatherPollingInterval`         |  Time in **Minutes** to check for changes in Weather. Default is `false` for no polling.       |             |  false |
| `occupancySensorsEnabled`        |  Enable **Occupancy Sensors**.  ***more details below*     |             |  false |
| `occupancyPollingInterval`       |  Time in **Seconds** to check for changes in occupnacy. Default is `10` for polling every 10 seconds. *it can't be set to false!*     |             |  10 |
| `anyoneSensor`       |  Adds 1 **Occupancy Sensor** named "Anyone" to represent the state of someone at home     |             |  true |

### * Outside Temperature & Solar Intensity Sensors
 Enabling this feature will add 2 new accessories to your home:

***All data is gathered from Tado API and is related specifically to your homes.***

**Outside Temperature** - Temperature sensor that will show you the temperature outside of your home area.

**Solar Intensity** - Light bulb accessory that will show you the relative(%) brightness of the sun in your home area. will go off when it's dark.

Those accessories are great for setting automation based on the weather condition, for example:
- When the first person arrive, If **Outside Temperature** is lower than 18ºC, Then, Turn on the heat.
- When **Solar Intensity** is off, Then, Turn on the Hall Lights & close Blinds.
- When **Solar Intensity** is lower then 20%, Then, Turn on the Garden Lights.

When setting automations that are based on these sensors, it's best to set a polling interval value so that the system will update Homekit for any changes in weather every X minutes. I reccomend to set it to 10 (for every 10 minutes).

When there are no automations based on those sensors it's best to set it to false (or remove from config) so it would be easier on the machine. That way, the status for the sensors will be retrieved only when the app is opened

### ** Occupancy Sensors
Enabling this feature will add **Occupancy Sensors** for each user signed up to your Tado home (and enabled location services on their device).
If not set otherwise, the system will check for the status every 10 seconds.

**"Anyone"** Sensor will be added automatically to easily automate actions when the first person arrives home or the last person leaves. this is a better alternative to Home App Arrive/Leave automations since this will not require approval for triggering automation. to remove this accessory, set `anyoneSensor` to false


## Troubleshooting

- Auto mode in the home app will show average temperature according to your maximum and minimum temperature, but because Tado app does not suport temperature for Auto mode, changing the temperature will not change anything in Tado.

- Rotation speed support every speed but not Auto speed since there is no option for that in homekit - therefor when rotation speed is set to Auto through Tado app it will just show middle-high speed in home app.

I tried my best to make this version flawless, but expect some issues since each user has his own different setup.
Once a new issue is noticed, please submit to [Issues](https://github.com/nitaybz/homebridge-tado-ac/issues)
