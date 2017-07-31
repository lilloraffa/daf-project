# Mobility & Transportation - Parking

The parking event describes the status of public parking in terms of free parking spot vs total available, measured in a given timeframe. This is a dataset that belongs to "Dataset Chiave", and defines a standard.

## Data Structure
Here are the rules to follow when compiling the generic event data structure in order to implement an event of type velocity. The fields not listed below does not have specific restrictions/configuration.

- `temporal_granularity`: TBD
- `event_type_id`: it needs to be set to `0` as this is an event described by a quantitative measure.
- `event_subtype_id`: it needs to be set to "mob_std_parking"
- `source`: TBD
- `attributes`: see paragraph below

### Attribute List
Here is the list of key/value pairs that MUST be defined in the `attributes` field.

- `measure`: [string] it is the name of the associated metric, and MUST be set to "parking"
- `value`: [integer] it is the quantitative value measured by the sensor. In this case the number of free parking spots.
- `tot_parking`: [integer] it is the number of total parking spots offered by the parking.
- `parking_name`: [string] it is the name of the parking to which the information refers to.
- `road`: [string] it is the name of the road where the sensor is placed. In case the road is not in a city, the field must contains the closest km at which the sensor has been placed (i.e. "SP101 - Km 150")
- `city`: [string, null] it is the name of the city where the sensor is located. In case the sensor is not located inside a city (i.e. it is on a "strada statale" or on a highway), then it will be "null"
- `prov`: [string] it is the name of the "Provincia" (or county) where the position of the sensor belongs to.
- `regione`: [string] the name of the `Regione` where the position of the sensor belongs to.

## Example
Here is an example of usage of the parking data structure to manage the specific event of subtype 'parking' under the Mobility & Transportation theme. The example is displayed in JSON format.

```
{
  "version": 1,
  "id": 12323123123,
  "ts": 1500909835,
  "temporal_granularity": "minute",
  "event_certainty": 1,
  "event_type_id": 0,
  "event_subtype_id": "mob_std_parking",
  "event_annotation": "Stato del parcheggio Bodoni, Torino.",
  "source": "source_id",
  "location": [(45.06355, 7.68357)],
  "body": null,
  "attributes": Map("metric"->"parking", "value"->204, "tot_parking"->400, "parking_name"->"Bodoni", "road": "via Bodoni 245", "city": "Torino", "prov"->"TO", "regione"->"Piemonte")
}
```
