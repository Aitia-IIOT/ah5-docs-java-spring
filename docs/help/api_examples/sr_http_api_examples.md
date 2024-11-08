# Service Registry | HTTP Examples

This file contains specific examples of what payloads are expected and returned by each endpoint maintained by the service registry.

## service-discovery

**Authentication:** For service related operations, the payload does not contain the system name, because it comes either from an authorization header or an X.509 certificate. 

The Service Registry generates the Service Instance ID from the system name. It also uses system name to identify who sent the request.

In the following examples, self-declared authentication is used, so the header should contain the system name with the  _SYSTEM//_ prefix. The system name will be _temperature-provider1_ in the examples.

~~~
-H 'Authorization: Bearer SYSTEM//temperature-provider1'
~~~

### register

**POST /serviceregistry/service-discovery/register**

##### Request body:

~~~
{
   "serviceDefinitionName":"celsius-info",
   "version":"1",
   "expiresAt":"2030-10-11T14:30:00Z",
   "metadata":{
      "frequency":400
   },
   "interfaces":[
      {
         "templateName":"generic-https",
         "protocol":"https",
         "policy":"NONE",
         "properties":{
            "accessAddresses":[
               "192.168.66.1"
            ],
            "accessPort":4040,
            "basePath":"/celsius-info",
            "operations":{
               "query-temperature":{
                  "method":"get",
                  "path":"/celsius-info"
               }
            }
         }
      }
   ]
}
~~~

##### Response Status: 

`200 OK`

##### Response Body:

~~~
{
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
         "templateName":"generic-https",
         "protocol":"https",
         "policy":"NONE",
         "properties":{
            "accessAddresses":[
               "192.168.66.1"
            ],
            "accessPort":4040,
            "operations":{
               "query-temperature":{
                  "path":"/celsius-info",
                  "method":"GET"
               }
            },
            "basePath":"/celsius-info"
         }
      }
   ],
   "createdAt":"2024-10-24T23:47:51.493025200Z",
   "updatedAt":"2024-10-24T23:47:51.493025200Z"
}
~~~

### lookup

**POST /serviceregistry/service-discovery/lookup?verbose=false**

##### Request Body:

~~~
{
   "instanceIds":[
      
   ],
   "providerNames":[
      "temperature-provider1"
   ],
   "serviceDefinitionNames":[
      "celsius-info"
   ],
   "versions":[
      "1.0.0",
      "1.0.1"
   ],
   "alivesAt":"",
   "metadataRequirementsList":[
      
   ],
   "interfaceTemplateNames":[
      "generic-http",
      "generic-https"
   ],
   "interfacePropertyRequirementsList":[
      
   ],
   "policies":[
      
   ]
}
~~~

##### Response Status:

`200 OK`

##### Response Body:
~~~
{
   "entries":[
      {
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
            "createdAt":"2024-11-03T20:20:50Z",
            "updatedAt":"2024-11-03T20:20:50Z"
         },
         "serviceDefinition":{
            "name":"celsius-info",
            "createdAt":"2024-10-24T21:48:36Z",
            "updatedAt":"2024-10-24T21:48:36Z"
         },
         "version":"1.0.0",
         "expiresAt":"2030-10-11T14:30:00Z",
         "metadata":{
            "unrestricted-discovery":true,
            "frequency":400
         },
         "interfaces":[
            {
               "templateName":"generic-https",
               "protocol":"https",
               "policy":"NONE",
               "properties":{
                  "accessAddresses":[
                     "192.168.66.1"
                  ],
                  "accessPort":4040,
                  "operations":{
                     "query-temperature":{
                        "path":"/celsius-info",
                        "method":"GET"
                     }
                  },
                  "basePath":"/celsius-info"
               }
            }
         ],
         "createdAt":"2024-11-03T20:21:05Z",
         "updatedAt":"2024-11-03T21:28:29Z"
      }
   ],
   "count":1
}
~~~

### revoke

**DELETE /serviceregistry/service-discovery/revoke/{instanceId}**
  
instanceId = temperature-provider1::celsius-info::1.1.0

##### Request

~~~
http://localhost:8443/serviceregistry/service-discovery/revoke/temperature-provider1%3A%3Acelsius-info%3A%3A1.1.0
~~~

##### Response Status: 

`200 OK`

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