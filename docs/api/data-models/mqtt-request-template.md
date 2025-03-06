# MqttRequestTemplate

Field | Type | Mandatory | Description
--- | --- | --- | --- 
traceId | String | no | Any kind of string choosen by the requester.
authentication | String | yes | Authentication related data.
responseTopic | String | yes | The topic on which the response is expected.
qosRequirement | [MQTTQoS](../primitives.md#mqttqos) | no | Required response MQTT Quality of Service.
params | Map<String,String> | no | Request parameters as string key-value pairs.
payload | Object | no | Any kind of data object.