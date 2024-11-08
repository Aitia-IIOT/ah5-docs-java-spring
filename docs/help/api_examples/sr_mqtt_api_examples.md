# Service Registry | MQTT Examples

## service-discovery

### register

Topic:

~~~
arrowhead/serviceregistry/service-discovery
~~~

Request message:

~~~
{
   "traceId":"7845",
   "operation":"register",
   "authentication":"SYSTEM//temperature-provider1",
   "responseTopic":"temperature-provider1/service-registry",
   "qosRequirement":2,
   "payload":{
      "serviceDefinitionName":"celsius-info",
      "version":"1",
      "expiresAt":"2030-10-11T14:30:00Z",
      "metadata":{
         "frequency":400
      },
      "interfaces":[
         {
            "templateName":"generic-mqtt",
            "protocol":"tcp",
            "policy":"NONE",
            "properties":{
               "accessAddresses":[
                  "192.168.66.1"
               ],
               "accessPort":4040,
               "topic":"temperature-provider1/celsius-info",
               "operations":[
                  "get-temperature"
               ]
            }
         }
      ]
   }
}
~~~

Response message:

~~~
{
   "status":200,
   "traceId":"7845",
   "receiver":"temperature-provider1",
   "payload":{
      "instanceId":"temperature-provider1::celsius-info::1.0.0",
      "provider":{
         "name":"temperature-provider1",
         "metadata":{
            "type":"temperature",
            "scales":[
               "Kelvin",
               "Celsius"
            ],
            "customizable":false
         },
         "version":"2.1.0",
         "addresses":[
            {
               "type":"IPV4",
               "address":"192.168.66.1"
            }
         ],
         "device":{
            "name":"thermometer1",
            "metadata":{
               "scales":[
                  "Kelvin",
                  "Celsius"
               ],
               "max-temperature":{
                  "Kelvin":310,
                  "Celsius":40
               },
               "min-temperature":{
                  "Kelvin":260,
                  "Celsius":-10
               },
               "appliable":[
                  "spring",
                  "autumn"
               ]
            },
            "addresses":[
               {
                  "type":"MAC",
                  "address":"81:ef:1a:44:7a:ff"
               }
            ],
            "createdAt":"2024-10-21T18:47:45Z",
            "updatedAt":"2024-10-21T18:47:45Z"
         },
         "createdAt":"2024-10-24T23:44:09Z",
         "updatedAt":"2024-10-24T23:44:09Z"
      },
      "serviceDefinition":{
         "name":"celsius-info",
         "createdAt":"2024-10-24T21:48:36Z",
         "updatedAt":"2024-10-24T21:48:36Z"
      },
      "version":"1.0.0",
      "expiresAt":"2030-10-11T14:30:00Z",
      "metadata":{
         "frequency":400
      },
      "interfaces":[
         {
            "templateName":"generic-mqtt",
            "protocol":"tcp",
            "policy":"NONE",
            "properties":{
               "accessAddresses":[
                  "192.168.66.1"
               ],
               "accessPort":4040,
               "operations":[
                  "query-temperature"
               ],
               "topic":"temperature-provider1/celsius-info"
            }
         }
      ],
      "createdAt":"2024-10-24T23:47:51.493025200Z",
      "updatedAt":"2024-10-24T23:47:51.493025200Z"
   }
}
~~~

### lookup
TODO

### revoke
TODO

## system-discovery

### register
TODO

### lookup
TODO

### revoke
TODO

## device-discovery

### register
TODO

### lookup
TODO

### revoke
TODO

## service-registry-management
TODO