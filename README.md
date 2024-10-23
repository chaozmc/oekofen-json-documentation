# Ökofen JSON Documentation
This is an attempt to publicy document the fields and features of the Ökofen JSON API as it lacks documentation from the vendorside. Please feel free to file pull requests in case you have any additions or corrections. Most of the field descriptions are the result of trial and error.

## Accessing the API
After enabling the JSON API you need to configure the port and password within the heater's settings menu. Afterwards the JSON API should be exposed and reachable. You are able to access all metrics by hitting the `all` endpoint of the API.

```
http://{$ip}:{$port}/{$password}/all
```

## Used Abbreviations

| Abbreviation | Meaning |
| ------------ | ----------- |
| hk{$n}       | Heating circuit #{$n} |
| pu{$n}       | Buffer tank #{$n} |
| ww{$n}       | Hot-water circuit #{$n} |
| sk{$n}       | Solar thermal collector circuit #{$n} |
| pe{$n}       | Pellematic heater #{$n} |
| se{$n}       | Solar gain data #{$n} |
| circ{$n}     | Hot-water circulation pump #{$n} |

## Fields
The name of the fields are described with the dot-notation syntax. For the field `json_data['system']['L_ambient']` the field name in this documentation would be `system.L_ambient`.

### system.L_ambient
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
Temperature that was reported by the ambient temperature sensor. You'll need to multiply the value by the given factor to get the actual temperature in degree celsius als floating point value.

### system.L_errors
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| None | 1      | int  | -32768 to 32767 |

**Description (Help Wanted)**  
Unclear if it's an error counter that is equal to the amount of errors that were detected or if the integer value that is reported by the API represents a specific error code or bitflag.

### system.L_usb_stick
| Unit | Factor | Type | Range  |
| ---- | ------ | ---- | ------ |
| None | None   | bool | 0 or 1 |

**Description (Unsure)**  
Whether an USB-stick is connected to the Pellematic heater or not.

### weather.L_temp
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
Current temperature that was reported by the online weather service (see `weather.L_source` for information about the configured weather provider).

### weather.L_clouds
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| %    | 1      | int  | -32768 to 32767 |

**Description**  
How cloudy it currently should be according to the configured online weather service (see `weather.L_source` for information about the configured weather provider).

### weather.L_forecast_temp

### weather.L_forecast_clouds

### weather.L_forecast_today

### weather.L_starttime

### weather.L_endtime

### weather.L_source

### weather.L_location

### weather.cloud_limit

### weather.hysteresys

### weather.offtemp

### weather.lead

### weather.refresh

### weather.oekomode

### forecast.L_w_{0-24}

### hk{$n}.L_roomtemp_act
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The current air temperature that was reported from a sensor that is installed inside a room that is heated by the used heating circuit. This value is zero (0) if there is no sensor installed for the given heating circuit.

### hk{$n}.L_roomtemp_set
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The target air temperature that is set by the user for the selected heating circuit. This value can be ignored if there is no sensor installed for the given heating circuit.

### hk{$n}.L_flowtemp_act
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The current flow temperature ("Vorlauftemperatur") of the heating system for the selected heating circuit.

### hk{$n}.L_flowtemp_set
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The target flow temperature ("Vorlauftemperatur") of the heating system for the selected heating circuit.

### hk{$n}.L_state

### hk{$n}.L_statetext

### hk{$n}.remote_override
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| K    | 0.1    | int  | -32768 to 32767 |

**Description**  
Influences the target room temperature. Basis is the datapoint temp_heat. This datapoint sets the wanted target to +X or -Y relative to the basis of temp_heat. For example, temp_heat is set to 22 degree and remote_override is set to +2 the real target for the heating system is 24 degree.

### hk{$n}.mode_auto
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| number | 1    | int  | 0 to 3          |

**Description**  
The current mode of the heating circuit. 0: Off, 1: Auto, 2: Heating, 3: Setback

### hk{$n}.time_prg
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| number | 1    | int  | 0 to 1          |

**Description**  
Defines the current active time program. Value 0 means "time program 1" and value 1 means "time program 2".

### hk{$n}.temp_setback  
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The current setback ("Absenktemperatur") temperature. Relevant if either the time program defines that current the setback time is active or if the circuit is set to mode 3.

### hk{$n}.temp_heat
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The target temperature for the LED room thermostate. This temperature is the target if the remote is set to "middle" position. Relevant for the data point remote_override

### hk{$n}.temp_vacation
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The target temperature while the heating system is in vacation mode. The vacation mode is set for a period of time on the touch (remote) control panel or through the app.

### hk{$n}.name
| Unit | Factor | Type   | Range |
| ---- | ------ | ------ | ----- |
| None | None   | string | None  |

**Description**  
You can set or get the name for the heating circuit

### hk{$n}.oekomode
| Unit | Factor | Type | Range                                     |
| ---- | ------ | ---- | ----------------------------------------- |
| num  | 1      | int  | 0:Off, 1:Comfort, 2:Minimum, 3:Ecological |

**Description**  
This setting has influence to the desired target temperature for the heating circuit. The system uses weather data to estimate if it get's warmer during daytime and adjusts the target temperatures accordingly. The setting itself expresses the influence of the weather to the heating circuit. If you set comfort here, then the heater respects the L_comfort setting. 

### pu{$n}.L_tpo_act

### pu{$n}.L_tpo_set

### pu{$n}.L_tpm_act

### pu{$n}.L_tpm_set

### pu{$n}.L_pump_release

### pu{$n}.L_pump

### pu{$n}.L_state

### pu{$n}.L_statetext

### pu{$n}.mintemp_off

### pu{$n}.mintemp_on

### pu{$n}.ext_mintemp_off

### pu{$n}.ext_mintemp_on

### ww{$n}.L_temp_set
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| °C   | 0.1    | int  | -32768 to 32767 |

**Description**  
The target temperature of the hot water boiler.

### ww{$n}.L_ontemp_act

### ww{$n}.L_offtemp_act

### ww{$n}.L_pump
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| num  | 1      | int  | 0 or 1          |

**Description**  
Indicates if the pump for the water heater is currently running or not. 0 means off, 1 means on.

### ww{$n}.L_state

### ww{$n}.L_statetext

| Unit | Factor | Type   | Range |
| ---- | ------ | ------ | ----- |
| n.a  | n.a    | string | n.a   |

**Description**  
The actual state of the hot water system. The heater tells if it's currently allowed to heat up water or not, and if/why water is heated or not. Also heat once is mentioned here. The whole string consists of all informations together and is seperated by a pipe (|) symbol.

### ww{$n}.time_prg
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| number | 1    | int  | 0 to 1          |

**Description**  
Defines the current active time program. Value 0 means "time program 1" and value 1 means "time program 2".


### ww{$n}.sensor_on

### ww{$n}.sensor_off

### ww{$n}.mode_auto

### ww{$n}.mode_dhw

### ww{$n}.heat_once
| Unit | Factor | Type | Range           |
| ---- | ------ | ---- | --------------- |
| None | None   | Bool | 0 or 1          |

**Description**  
If the time program normally would not allow the system to heat the water boiler, this value overrides the behaviour and allows the water to be heated once. This mode is set back after the hot water boiler reaching the target temperature.

### ww{$n}.temp_min_set

### ww{$n}.temp_max_set

### ww{$n}.name

| Unit | Factor | Type   | Range |
| ---- | ------ | ------ | ----- |
| None | None   | string | None  |

**Description**  
You can set or get the name for the hot water circuit

### ww{$n}.smartstart

| Unit    | Factor | Type | Range |
| ------- | ------ | ---- | ----- |
| minutes | 1      | int  | 0-90  |

**Description**  
Allowes the heater to start X minutes earlier in the morning if the water temperature is low enough and according to weather forecast there's no sun for solar water heating.

### ww{$n}.use_boiler_heat

| Unit | Factor | Type | Range        |
| ---- | ------ | ---- | ------------ |
| num  | 1      | int  | 0:Off , 1:On |

**Description**  
When the heater goes to Off, setting this to 1 (On) will use the remaining heat in the system to (over-)heat the water boiler so the remaining thermal energy isn't wasted, instead it's stored in the warm water.

### ww{$n}.oekomode

| Unit | Factor | Type | Range                                     |
| ---- | ------ | ---- | ----------------------------------------- |
| num  | 1      | int  | 0:Off, 1:Comfort, 2:Minimum, 3:Ecological |

**Description**  
Setting this influences when water will be allowed to be heated. The system takes the weather forecast into consideration. The range is from 0 (don't use the weather forecast at all) to 3 (strongly let the forecast influence the beginning of the start time). Using 3 means also during summer that the system locks the warm water at all if sun is expected around noon. This will be announced in the L_statetext with a message like "Sunny weather expected, warm water setpoint reduced".

### sk{$n}.L_koll_temp

### sk{$n}.L_spu

### sk{$n}.L_pump

### sk{$n}.L_state

### sk{$n}.L_statetext

### sk{$n}.mode

### sk{$n}.cooling

### sk{$n}.spu_max

### sk{$n}.name

### se{$n}.L_flow_temp

### se{$n}.L_ret_temp

### se{$n}.L_flow

### se{$n}.L_pwr

### se{$n}.L_counter

### se{$n}.L_total

### se{$n}.L_day

### se{$n}.L_yesterday

### circ{$n}.L_pump

### circ{$n}.L_ret_temp

### circ{$n}.L_release_temp

### circ{$n}.time_prg

### circ{$n}.mode

### circ{$n}.pump_release

### circ{$n}.return_set

### circ{$n}.name

### pe{$n}.L_temp_act

### pe{$n}.L_temp_set

### pe{$n}.L_ext_temp

### pe{$n}.L_frt_temp_act

### pe{$n}.L_frt_temp_set

### pe{$n}.L_br

### pe{$n}.L_ak

### pe{$n}.L_not

### pe{$n}.L_pellets_today

| Unit    | Factor | Type | Range         |
| ------- | ------ | ---- | ------------- |
|   kg    | 1      | int  | -32768-32767  |

**Description**  
This parameter indicates the amount of pellets used today sofar, measured in kilograms (kg). "Pelletverbrauch heute"

### pe{$n}.L_pellets_yesterday

| Unit    | Factor | Type | Range         |
| ------- | ------ | ---- | ------------- |
|   kg    | 1      | int  | -32768-32767  |

**Description**  
This parameter indicates the amount of pellets used yesterday, measured in kilograms (kg). "Pelletverbrauch gestern"

### pe{$n}.L_resttimeburner

| Unit    | Factor | Type | Range         |
| ------- | ------ | ---- | ------------- |
|   zs    | 0.01   | int  | ?             |

**Description**  
? "Pausenzeit"

### pe{$n}.L_runtimeburner

| Unit    | Factor | Type | Range         |
| ------- | ------ | ---- | ------------- |
|   zs    | 0.01   | int  | ?             |

**Description**  
This parameter indicates the total time the burner was running. "Einschubzeit"


### pe{$n}.L_stb

| Unit    | Factor | Type    | Range          |
| ------- | ------ | ------- | -------------- |
|  none   |  none  | string  | {0:Aus, 1:Ein} |

**Description**  
This parameter indicates stus of the "Safety Thermostat".


### pe{$n}.L_storage_fill

| Unit    | Factor | Type | Range    |
| ------- | ------ | ---- | -------- |
|   kg    | 1      | int  | 0-32767  |

**Description**  
This parameter indicates the amount of pellets remaining in the local storage unit, measured in kilograms (kg). It provides a value in integer form with a range from 0 to 32767 kg.

### pe{$n}.L_storage_hopper

| Unit    | Factor | Type | Range          |
| ------- | ------ | ---- | -------------- |
|   kg    | 1      | int  | -327680-32767  |

**Description**  
This parameter indicates the amount of pellets remaining in the intermediate container "Zwischenbehälter", measured in kilograms (kg). It provides a value in integer form with a range from -32680 to 32767 kg.


### pe{$n}.L_storage_max

| Unit    | Factor | Type | Range        |
| ------- | ------ | ---- | ------------ |
|   kg    | 1      | int  | 150 - 30000  |

**Description**  
This parameter indicates the max amount of pellets in the local storage unit "Füllstand Lager", measured in kilograms (kg). It provides a value in integer form with a range from 150 to 30000 kg.


### pe{$n}.L_storage_min

| Unit    | Factor | Type | Range       |
| ------- | ------ | ---- | ----------- |
|   kg    | 1      | int  | 0 - 4000    |

**Description**  
This parameter indicates the min amount of pellets in the local storage unit "warnung bei" and can trigger a notification when storage is below defined value. It is measured in kilograms (kg). It provides a value in integer form with a range from 0 to 4000 kg.

### pe{$n}.L_storage_popper

not supported in Version 4.04?


### pe{$n}.L_modulation
| Unit    | Factor | Type | Range             |
| ------- | ------ | ---- | ----------------- |
|   %     | 1      | int  | -32768 - 32767    |

**Description**  
This parameter indicates Modulationsstufe. (Did not observe any other value than 1 or 0)

### pe{$n}.L_uw_speed

### pe{$n}.L_state

| Unit    | Factor | Type    | Range                    |
| ------- | ------ | ------- | ------------------------ |
|  none   |  1     | string  | {16:Leistungsbrand, x:y} |

**Description**  
This parameter indicates status of the burner (Kesselstatus)

### pe{$n}.L_statetext

### pe{$n}.L_type

### pe{$n}.L_starts

### pe{$n}.L_runtime

### pe{$n}.L_avg_runtime

### pe{$n}.L_uw_release

### pe{$n}.L_uw

### pe{$n}.mode

### error
