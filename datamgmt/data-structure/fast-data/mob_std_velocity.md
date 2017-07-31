# Mobility & Transportation - Velocity

The velocity event describes the average speed, measured in a given timeframe, generated by a sensor. This is a dataset that belongs to "Dataset Chiave", and defines a standard.

## Data Structure
Here are the rules to follow when compiling the generic event data structure in order to implement an event of type velocity. The fields not listed below does not have specific restrictions/configuration.

- `temporal_granularity`: TBD
- `event_type_id`: it needs to be set to `0` as this is an event described by a quantitative measure.
- `event_subtype_id`: it needs to be set to "mob_std_velocity"
- `source`: TBD
- `attributes`: see paragraph below

### Attribute List
Here is the list of key/value pairs that MUST be defined in the `attributes` field.

- `measure`: [string] it is the name of the associated metric, and MUST be set to "velocity"
- `value`: [double] it is the quantitative value measured by the sensor.
- `road_direction_val`: [integer] it indicates which direction of the road the velocity refers to. It can be either "1" or "-1"
- `road_name`: [string] it is the name of the road where the sensor is placed. In case the road is not in a city, the field must contains the closest km at which the sensor has been placed (i.e. "SP101 - Km 150")
- `city`: [string, null] it is the name of the city where the sensor is located. In case the sensor is not located inside a city (i.e. it is on a "strada statale" or on a highway), then it will be "null"
- `prov`: [string] it is the name of the "Provincia" (or county) where the position of the sensor belongs to.
- `regione`: [string] the name of the `Regione` where the position of the sensor belongs to.

## Example
Here is an example of usage of the event data structure to manage the specific event of subtype 'velocity' under the Mobility & Transportation theme. The example is displayed in JSON format.

```
{
  "version": 1,
  "id": 12323123123,
  "ts": 1500909835,
  "temporal_granularity": "minute",
  "event_certainty": 1,
  "event_type_id": 0,
  "event_subtype_id": "mob_std_velocity",
  "event_annotation": "Traffico sensore azsccf di via bissolati",
  "source": "source_id",
  "location": [(45.464211, 9.191383)],
  "body": null,
  "attributes": Map("metric"->"velocity", "value"->40.3, "direction"->1, "offset"->687, "road_name": "via bissolati", "city": "Milano", "prov"->"MI", "regione"->"Lombardia")
}
```