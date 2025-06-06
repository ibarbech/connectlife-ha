# ConnectLife

ConnectLife integration for Home Assistant

[![BuyMeCoffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/oyvindwev)

## Installation

You can install using HACS or download.

### HACS
If you have HACS installed, add this repository (`oyvindwe/connectlife-ha`) as a custom repository of type "Integration."

See https://hacs.xyz/docs/faq/custom_repositories/ 

### Download

Download the `connectlife` directory and place in your `<config>/custom_components/`.

After installing, you need to restart Home Assistant.

Finally, add "ConnectLife" as an integration in the UI, and provide the username and password for your ConnectLife account.

Your device and all their status values should show up.

## Supported ConnectLife devices

See [DEVICES.md](DEVICES.md) for the full list of supported devices.

### Default device types

Default mapping files are provided for the following device types:

| Device type              | Device type code                                                |
|--------------------------|-----------------------------------------------------------------|
| Portable air conditioner | [006](custom_components/connectlife/data_dictionaries/006.yaml) |
| Dehumidifier             | [007](custom_components/connectlife/data_dictionaries/007.yaml) |
| Window air conditioner   | [008](custom_components/connectlife/data_dictionaries/008.yaml) |
| Air conditioner          | [009](custom_components/connectlife/data_dictionaries/009.yaml) |
| Hood                     | [012](custom_components/connectlife/data_dictionaries/012.yaml) |
| Dishwasher               | [015](custom_components/connectlife/data_dictionaries/015.yaml) |
| Heat pump                | [016](custom_components/connectlife/data_dictionaries/016.yaml) |
| Induction hob            | [020](custom_components/connectlife/data_dictionaries/020.yaml) |
| Oven                     | [023](custom_components/connectlife/data_dictionaries/023.yaml) |
| Washing machine          | [027](custom_components/connectlife/data_dictionaries/027.yaml) |
| Tumble dryer             | [030](custom_components/connectlife/data_dictionaries/030.yaml) |
| Tumble dryer             | [032](custom_components/connectlife/data_dictionaries/032.yaml) |

Any devices of these types will use the default mapping file, but it may not be fully functional until a
feature-specific mapping file is provided.

Any unmapped properties will show up as sensors with names based on their properties. As there are a lot of exposed
sensors, all unknown sensors are hidden by default. Access the device or entity list to view sensors and change
visibility.

Please contribute PRs with [mapping files](custom_components/connectlife/data_dictionaries) for your devices!

## Disable beeping

Some devices will beep on every configuration change. To disable this, go to the
[ConnectLife integration](https://my.home-assistant.io/redirect/integration/?domain=connectlife)
and click "Configure" → "Configure a device" and select the device you want to disable beeping for. 

## Service to set property values on sensors

Entity service `connectlife.set_value` can be used to set values. Use with caution, as there is **no** validation
if property is writeable or that the value is legal to set.

1. The service can be accessed from [Developer tools - Services](https://my.home-assistant.io/redirect/developer_services/).
2. Search for service name "ConnectLife: Set value"
3. Select entity as target.
4. Enter value
5. Call service.

It is possible to guard against `set_value` by setting `read_only: true` in the data dictionary on the sensor, e.g.
```yaml
  - property: f_status 
    sensor:
      read_only: true
```

## Issues

### Climate entities

Please ignore the following warning in the log:
```
Entity None (<class 'custom_components.connectlife.climate.ConnectLifeClimate'>) implements HVACMode(s): auto, off and therefore implicitly supports the turn_on/turn_off methods without setting the proper ClimateEntityFeature. Please report it to the author of the 'connectlife' custom integration
```

Missing features:
- Setting `target_temperature_high`/`target_temperature_low`

### Heat pump entities
 
Missing features:
- Setting state except to off/one defined state
- Setting `target_temperature_high`/`target_temperature_low`

### Login

You have to create a ConnectLife account with username/password. SSO using other identity providers is not supported (See https://github.com/oyvindwe/connectlife-ha/issues/99).

To work around this issue, you can do the following:
1. In the ConnectLife mobile app, create a new account using an email and password (needs to be a different email to your SSO account as that is now occupied by that SSO).
2. Still in the mobile app, switch accounts back to your SSO account, go to the device/s you have already setup in your SSO account, select them 1 by 1, scrolling to the bottom to get to the "Share Device" option and adding your new email to the shared accounts list.
3. In the connectlife-ha integration within Home Assistant, use the email and password from the new account you set up, which will now have the device(s) shared with it.
4. Forget your new email version of your account exists, you don't have to accept device shares or use that account for any other reason.

Note that users at least in Russia and China can't log in using this integration. See discussion in
https://github.com/bilan/connectlife-api-connector/issues/25
