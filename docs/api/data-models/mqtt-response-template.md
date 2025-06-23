# MqttResponseTemplate

Field | Type | Description
--- | --- | --- 
status | Integer | Response status code. 
traceId | String | The string identifier given by the requester.
receiver | [SystemName](../primitives.md#systemname) | Unique identifier of the system to which the response message is addressed.
payload | Object | Any kind of data object.