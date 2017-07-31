# DataStructure - Real-Time events and streaming

The ingestion of real-time events & streaming is treated with a general purpose data structure that should be able to adapt to the following use cases:

- events characterized by a quantitative information (i.e. average velocity recorded by a sensor in a given timeframe);
- events characterized by a qualitative information (i.e. the details of an car accident happened at a specific point on the highway).

The idea is to have a unique "generic" data structure to handle all kind of event streaming, and apply rules to the content of the message to manage specific type of event.

## Event Data Structure
As mentioned, this is the data structure used to manage every kind of event sent to the system. The specificities of each event type (i.e. velocity, traffic flux, info & warning from citizens, etc.), each of which needing specific attributes.

Avro Schema:
```
{
	"namespace": "it.gov.daf.iotingestion.event",
	"name": "Event",
	"type": "record",
	"doc": "A generic event. See the reference guide for event format information.",
	"version": 1,
	"fields": [{
			"name": "version",
			"type": "long",
			"doc": "Version of this schema",
			"default": 1
		},
		{
			"name": "id",
			"type": ["null", "string"],
			"doc": "A globally unique identifier for this event. Optional",
			"default": null
		},
		{
			"name": "ts",
			"type": "long",
			"doc": "Epoch timestamp in millis. Required."
		},
		{
			"name": "temporal_granularity",
			"type": ["null", "string"],
			"doc": "atom of time from a particular application’s point of view, for example: second, minute, hour, or day. Optional.",
			"default": null
		},
		{
			"name": "event_certainty",
			"type": "double",
			"doc": "estimation of the certainty of this particular event [0,1]. Optional.",
			"default": 1.0
		},
		{
			"name": "event_type_id",
			"type": "int",
			"doc": "ID indicating the type of event: 0 for metric event, 1 for changing state events, 2 for generic events Required."
		},
    {
			"name": "event_subtype_id",
			"type": ["null", "string"],
			"doc": "String indicating the specific type of event to which the instance refers to. This will tell the system what info and structure to expect from the attribute list.",
      "default": null
		},
		{
			"name": "event_annotation",
			"type": ["null", "string"],
			"doc": "free-text explanation of what happened in this particular event. Optional.",
			"default": null
		},
		{
			"name": "source",
			"type": "string",
			"doc": "The event source attribute is the name of the entity that originated this event. This can be either an event producer or an event processing agent. Required"

		},
		{
			"name": "location",
			"type": "string",
			"doc": "Location from which the event was generated. Required.",
			"default": ""
		},
		{
			"name": "body",
			"type": ["null", "bytes"],
			"doc": "Raw event content in bytes. Optional.",
			"default": null
		},
		{
			"name": "attributes",
			"type": {
				"type": "map",
				"values": "string"
			},
			"doc": "Event type-specific key/value pairs, usually extracted from the event body. The list of attributes is dictated by the specific event_subtype_id specified. Required.",
			"order": "ignore"
		}
	]
}
```

## Example of specific event
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
  "attributes": Map("metric"->"velocity", "value"->40.3, "direction"->1, "offset"->687, "road_name": "via bissolati", "city_name": "MI")
}
```
