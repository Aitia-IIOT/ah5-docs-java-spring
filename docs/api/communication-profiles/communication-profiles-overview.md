# Communication Profiles

A Communication Profile (CP) defines the rules to follow and the properties to apply to a given communication protocol when specifying a [service interface](../../help/definitions.md#service-interface).

This Arrowhead FW implementation is offering the following CPs to interact with services of the Core/Support systems and also to apply by the provider systems developers:

| CP | Security|
| --- | --- |
| [Generic HTTP](./generic-http-template.md) | Not secured |
| [Generic HTTPS](./generic-https-template.md) | Encrypted data transfer | 
| [Generic MQTT](./generic-mqtt-template.md) | Not secured |  
| [Generic MQTTS](./generic-mqtts-template.md) | Encrypted data transfer | 

> **Important!** These CPs are required only for interactions with the Arrowhead Framework. Consumer and provider systems must use one of the defined profiles when communicating with an Arrowhead Core or Support system. However, this requirement does not apply to the communication between the consumer and provider systems themselves — **they are free to use any communication method or protocol that best suits their needs**.