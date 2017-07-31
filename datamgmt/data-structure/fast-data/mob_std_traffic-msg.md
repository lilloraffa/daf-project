# Mobility & Transportation - Traffic Messages
The traffic message events contains real-time qualitative information on viability. This is a dataset that belongs to "Dataset Chiave", and defines a standard.

## Data Structure
Here are the rules to follow when compiling the generic event data structure in order to implement an event of type velocity. The fields not listed below does not have specific restrictions/configuration.

- `temporal_granularity`: TBD
- `event_type_id`: it needs to be set to `1` as this is an event described by a qualitative information.
- `event_subtype_id`: it needs to be set to "mob_std_traffic-msg"
- `source`: TBD
- `attributes`: see paragraph below
- `body`: it contains a `struct` with the following required information (it may contains additional optional info).

### Attribute List
Here is the list of key/value pairs that MUST be defined in the `attributes` field.

- `measure`: [string] it is the name of the associated metric, and MUST be set to "velocity"
- `value`: [double] it is the quantitative value measured by the sensor.
- `direction`: [integer] it indicates which direction of the road the velocity refers to. It can be either "1" or "-1"
- `road_name`: [string] it is the name of the road where the sensor is placed. In case the road is not in a city, the field must contains the closest km at which the sensor has been placed (i.e. "SP101 - Km 150")
- `city`: [string, null] it is the name of the city where the sensor is located. In case the sensor is not located inside a city (i.e. it is on a "strada statale" or on a highway), then it will be "null"
- `prov`: [string] it is the name of the "Provincia" (or county) where the position of the sensor belongs to.
- `regione`: [string] the name of the `Regione` where the position of the sensor belongs to.
