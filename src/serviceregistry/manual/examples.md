# **Examples of data transfer objects sent and received**

This file contains specific examples of what payloads are expected and returned by each endpoint maintained by the ServiceRegistry.

# Discovery endpoints

## **deviceDiscovery**

- **Register device:** POST /serviceregistry/device-discovery/register
  
  Request body: 
  ~~~
  {
    "name": "THERMOMETER1",
    "metadata": {
      "scales": ["Kelvin", "Celsius"],
      "maxTemperature": {"Kelvin": 310, "Celsius": 40},
      "minTemperature": {"Kelvin": 260, "Celsius": -10},
      "appliable": ["spring", "autumn"]
    },
    "addresses": [
        "81:ef:1a:44:7a:ff"
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "name": "THERMOMETER1",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "maxTemperature": {
        "Kelvin": 310,
        "Celsius": 40
      },
      "minTemperature": {
        "Kelvin": 260,
        "Celsius": -10
      },
      "appliable": [
        "spring",
        "autumn"
      ]
    },
    "addresses": [
      {
        "type": "MAC",
        "address": "81:ef:1a:44:7a:ff"
      }
    ],
    "createdAt": "2024-10-21T18:47:45.388404800Z",
    "updatedAt": "2024-10-21T18:47:45.388404800Z"
  }
  ~~~
  
- **Lookup device:** POST /serviceregistry/device-discovery/lookup
  
  Request body:
  ~~~
  {
    "deviceNames": [
    ],
    "addresses": [
      "81:ef:1a:44:7a:ff", "ab:ff:c8:e1:45:9a"
    ],
    "addressType": "MAC",
    "metadataRequirementList": [
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "THERMOMETER1",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "maxTemperature": {
            "Kelvin": 310,
            "Celsius": 40
          },
          "minTemperature": {
            "Kelvin": 260,
            "Celsius": -10
          },
          "appliable": [
            "spring",
            "autumn"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "81:ef:1a:44:7a:ff"
          }
        ],
        "createdAt": "2024-10-21T18:47:45Z",
        "updatedAt": "2024-10-21T18:47:45Z"
      },
      {
        "name": "WEATHER_DISPLAYER1",
        "metadata": {
          "type": "analogue",
          "displayedData": [
            "temperature"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9a"
          }
        ],
        "createdAt": "2024-10-22T09:44:20Z",
        "updatedAt": "2024-10-22T09:44:20Z"
      }
    ],
    "count": 2
  }
  ~~~

- **Revoke device:** DELETE /serviceregistry/device-discovery/revoke/{name}
  
  Path parameter: TEST_WEATHER_DISPLAYER
  ~~~
  http://localhost:8443/serviceregistry/device-discovery/revoke/TEST_WEATHER_DISPLAYER
  ~~~

  Response code: 200 

##  **systemDiscovery**

**Authentication:** For system related operations, the payload does not contain the system name, because it comes either from an authorization header or an X.509 certificate. 

In the following examples, self-declared authentication is used, so the header should contain the system name with the  _SYSTEM//_ prefix. The system name will be _TemperatureProvider1_ in the examples. This should be changed to the name of the actual system.

  ~~~
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider1' \
  ~~~

The header is also included in the examples where the system name comes from it.

- **Register system:** POST /serviceregistry/system-discovery/register

  Header:
  ~~~
  curl -X 'POST' \
    'http://localhost:8443/serviceregistry/system-discovery/register' \
    -H 'accept: application/json' \
    -H 'Authorization: Bearer SYSTEM//TemperatureProvider1' \
    -H 'Content-Type: application/json' \
  ~~~
  Request body:
  ~~~
  {
    "metadata": {
      "type": "temperature",
      "scales": [ "Kelvin", "Celsius" ],
      "customizable": false
    },
    "version": "2.1",
    "addresses": [ "tp1.greenhouse.com", "192.168.66.1" ],
    "deviceName": "THERMOMETER1"
  }
  ~~~

  Response body:
  ~~~
  {
    "name": "TemperatureProvider1",
    "metadata": {
      "type": "temperature",
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "customizable": false
    },
    "version": "2.1.0",
    "addresses": [
      {
        "type": "HOSTNAME",
        "address": "tp1.greenhouse.com"
      },
      {
        "type": "IPV4",
        "address": "192.168.66.1"
      }
    ],
    "device": {
      "name": "THERMOMETER1",
      "metadata": {
        "scales": [
          "Kelvin",
          "Celsius"
        ],
        "maxTemperature": {
          "Kelvin": 310,
          "Celsius": 40
        },
        "minTemperature": {
          "Kelvin": 260,
          "Celsius": -10
        },
        "appliable": [
          "spring",
          "autumn"
        ]
      },
      "addresses": [
        {
          "type": "MAC",
          "address": "81:ef:1a:44:7a:ff"
        }
      ],
      "createdAt": "2024-10-21T18:47:45Z",
      "updatedAt": "2024-10-21T18:47:45Z"
    },
    "createdAt": "2024-10-23T21:33:04.585419100Z",
    "updatedAt": "2024-10-23T21:33:04.585419100Z"
  }
  ~~~

- **Lookup system:** /serviceregistry/system-discovery/lookup
  
  _Query parameter: verbose=true_

  Request body:
  ~~~
  {
    "systemNames": [
    ],
    "addresses": [
    ],
    "addressType": "",
    "metadataRequirementList": [
      {
        "customizable": true,
        "type": "temperature",
        "scales": { "op": "CONTAINS", "value": "Celsius" }
      }
    ],
    "versions": [
    ],
    "deviceNames": [
      "THERMOMETER1", "THERMOMETER2", "THERMOMETER3", "THERMOMETER5"
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "MainTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": [
            "Fahrenheit",
            "Kelvin",
            "Celsius"
          ]
        },
        "version": "2.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          },
          {
            "type": "HOSTNAME",
            "address": "mtp.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER5",
          "metadata": {
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ],
            "maxTemperature": {
              "Kelvin": 310,
              "Celsius": 40,
              "Fahrenheit": 140
            },
            "minTemperature": {
              "Fahrenheit": 20,
              "Kelvin": 260,
              "Celsius": -10
            },
            "appliable": [
              "spring",
              "summer",
              "autumn",
              "winter"
            ]
          },
          "addresses": [
            {
              "type": "MAC",
              "address": "81:ef:1a:44:7b:02"
            }
          ],
          "createdAt": "2024-10-22T10:34:10Z",
          "updatedAt": "2024-10-22T10:34:10Z"
        },
        "createdAt": "2024-10-23T21:43:35Z",
        "updatedAt": "2024-10-23T21:43:35Z"
      }
    ],
    "count": 1
  }
  ~~~
  
- **Revoke system:** DELETE 
/serviceregistry/system-discovery/revoke

Header:
~~~
curl -X 'DELETE' \
  'http://localhost:8443/serviceregistry/system-discovery/revoke' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider1'
~~~

~~~
http://localhost:8443/serviceregistry/system-discovery/revoke
~~~

Reponse code: 200 

## **serviceDiscovery**

**Authentication:** For service related operations, the payload does not contain the system name, because it comes either from an authorization header or an X.509 certificate. 

The ServiceRegistry generates the service instance ID from the system name. It also uses system name to identify who sent the request.

In the following examples, self-declared authentication is used, so the header should contain the system name with the  _SYSTEM//_ prefix. The system name will be _TemperatureProvider1_ in the examples.

  ~~~
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider1' \
  ~~~

- **Register service:** POST /serviceregistry/service-discovery/register
  
  Request body:
  ~~~
  {
    "serviceDefinitionName": "celsiusInfo",
    "version": "1",
    "expiresAt": "2030-10-11T14:30:00Z",
    "metadata": {
      "frequency": 400
    },
    "interfaces": [
      {
        "templateName": "generic_https",
        "protocol": "https",
        "policy": "NONE",
        "properties": {
          "accessAddresses": ["192.168.66.1"],
          "accessPort": 4040,
          "basePath": "/celsius-info",
          "operations": {"query-temperature": { "method": "get", "path": "/celsius-info"} }
        }
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "instanceId": "TemperatureProvider1|celsiusInfo|1.0.0",
    "provider": {
      "name": "TemperatureProvider1",
      "metadata": {
        "type": "temperature",
        "scales": [
          "Kelvin",
          "Celsius"
        ],
        "customizable": false
      },
      "version": "2.1.0",
      "addresses": [
        {
          "type": "IPV4",
          "address": "192.168.66.1"
        }
      ],
      "device": {
        "name": "THERMOMETER1",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "maxTemperature": {
            "Kelvin": 310,
            "Celsius": 40
          },
          "minTemperature": {
            "Kelvin": 260,
            "Celsius": -10
          },
          "appliable": [
            "spring",
            "autumn"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "81:ef:1a:44:7a:ff"
          }
        ],
        "createdAt": "2024-10-21T18:47:45Z",
        "updatedAt": "2024-10-21T18:47:45Z"
      },
      "createdAt": "2024-10-24T23:44:09Z",
      "updatedAt": "2024-10-24T23:44:09Z"
    },
    "serviceDefinition": {
      "name": "celsiusInfo",
      "createdAt": "2024-10-24T21:48:36Z",
      "updatedAt": "2024-10-24T21:48:36Z"
    },
    "version": "1.0.0",
    "expiresAt": "2030-10-11T14:30:00Z",
    "metadata": {
      "frequency": 400
    },
    "interfaces": [
      {
        "templateName": "generic_https",
        "protocol": "https",
        "policy": "NONE",
        "properties": {
          "accessAddresses": [
            "192.168.66.1"
          ],
          "accessPort": 4040,
          "operations": {
            "query-temperature": {
              "path": "/celsius-info",
              "method": "GET"
            }
          },
          "basePath": "/celsius-info"
        }
      }
    ],
    "createdAt": "2024-10-24T23:47:51.493025200Z",
    "updatedAt": "2024-10-24T23:47:51.493025200Z"
  }
  ~~~
  
- **Lookup service:** POST /serviceregistry/service-discovery/lookup

  _Query parameter: verbose=false_

  Request body:
  ~~~
  {
    "instanceIds": [ 
    ],
    "providerNames": [
        "TemperatureProvider1" 
    ],
    "serviceDefinitionNames": [
        "celsiusInfo"
    ],
    "versions": [
        "1.0.0", "1.0.1"
    ],
    "alivesAt": "",
    "metadataRequirementsList": [
    ],
    "interfaceTemplateNames": [
        "generic_http", "generic_https"
    ],
    "interfacePropertyRequirementsList": [
    ],
    "policies": [
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "instanceId": "TemperatureProvider1|celsiusInfo|1.0.0",
        "provider": {
          "name": "TemperatureProvider1",
          "metadata": {
            "type": "temperature",
            "scales": [
              "Kelvin",
              "Celsius"
            ],
            "customizable": false
          },
          "version": "2.1.0",
          "createdAt": "2024-11-03T20:20:50Z",
          "updatedAt": "2024-11-03T20:20:50Z"
        },
        "serviceDefinition": {
          "name": "celsiusInfo",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "unrestrictedDiscovery": true,
          "frequency": 400
        },
        "interfaces": [
          {
            "templateName": "generic_https",
            "protocol": "https",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.66.1"
              ],
              "accessPort": 4040,
              "operations": {
                "query-temperature": {
                  "path": "/celsius-info",
                  "method": "GET"
                }
              },
              "basePath": "/celsius-info"
            }
          }
        ],
        "createdAt": "2024-11-03T20:21:05Z",
        "updatedAt": "2024-11-03T21:28:29Z"
      }
    ],
    "count": 1
  }
  ~~~

- **Revoke service:** DELETE /serviceregistry/service-discovery/revoke/{instanceid}
  
  Path parameter: TemperatureProvider1|CcelsiusInfo|1.1.0

  ~~~
  http://localhost:8443/serviceregistry/service-discovery/revoke/TemperatureProvider1%7CcelsiusInfo%7C1.1.0
  ~~~

  Response code: 200

## **Logs**

POST /serviceregistry/mgmt/logs

  Request body:
  ~~~
  {
    "pagination": {
      "page": 0,
      "size": 5,
      "direction": "ASC",
      "sortField": "entryDate"
    },
    "from": "2024-10-23T00:00:00Z",
    "to": "2024-10-24T00:00:00Z",
    "severity": "DEBUG",
    "logger": "eu.arrowhead.serviceregistry.init.ServiceRegistryApplicationInitListener"
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "logId": "11e0a3ef-9184-11ef-9618-0a0027000004",
        "entryDate": "2024-10-23T21:16:29.318Z",
        "logger": "eu.arrowhead.serviceregistry.init.ServiceRegistryApplicationInitListener",
        "severity": "INFO",
        "message": "Core system eu.arrowhead.serviceregistry.ServiceRegistrySystemInfo@2404abe2 revoked 0 service(s)."
      },
      {
        "logId": "1b072b68-9184-11ef-8015-0a0027000004",
        "entryDate": "2024-10-23T21:16:44.685Z",
        "logger": "eu.arrowhead.serviceregistry.init.ServiceRegistryApplicationInitListener",
        "severity": "INFO",
        "message": "System name: serviceregistry"
      },
      {
        "logId": "1b092739-9184-11ef-8015-0a0027000004",
        "entryDate": "2024-10-23T21:16:44.688Z",
        "logger": "eu.arrowhead.serviceregistry.init.ServiceRegistryApplicationInitListener",
        "severity": "INFO",
        "message": "SSL mode: DISABLED"
      },
      {
        "logId": "1b0c0d6a-9184-11ef-8015-0a0027000004",
        "entryDate": "2024-10-23T21:16:44.692Z",
        "logger": "eu.arrowhead.serviceregistry.init.ServiceRegistryApplicationInitListener",
        "severity": "INFO",
        "message": "Authentication policy: DECLARED"
      },
      {
        "logId": "1b8cd62b-9184-11ef-8015-0a0027000004",
        "entryDate": "2024-10-23T21:16:45.566Z",
        "logger": "eu.arrowhead.serviceregistry.init.ServiceRegistryApplicationInitListener",
        "severity": "INFO",
        "message": "System serviceregistry published 4 service(s)."
      }
    ],
    "count": 5
  }
  ~~~

## **Config**

GET /serviceregistry/mgmt/get-config

Query parameters: service.discovery.policy, discovery.verbose

~~~
http://localhost:8443/serviceregistry/mgmt/get-config?keys=service.discovery.policy&keys=discovery.verbose
~~~

Response body:
~~~
  {
    "map": {
      "discovery.verbose": "true",
      "service.discovery.policy": "restricted"
    }
  }
~~~

# Monitor endpoint
GET /serviceregistry/monitor/echo

Response body:

~~~
Got it!
~~~

# Management endpoints

## **Device-related**

- **Create devices:** POST /serviceregistry/mgmt/devices
  
  Request body:
  ~~~
  {
    "devices": [
      {
        "name": "WEATHER_DISPLAYER1",
        "metadata": {
          "type": "analogue",
          "displayedData": [ "temperature" ]
        },
        "addresses": [ "ab:ff:c8:e1:45:9a" ]
      },
      {
        "name": "WAETHER_DISPLAYER2",
        "metadata": {
          "type": "digital",
          "displayedData": [ "wind", "humidity", "temperature" ]
        },
        "addresses": [ "ab:ff:c8:e1:45:9b" ]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "WEATHER_DISPLAYER1",
        "metadata": {
          "type": "analogue",
          "displayedData": [
            "temperature"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9a"
          }
        ],
        "createdAt": "2024-10-22T09:44:19.658404700Z",
        "updatedAt": "2024-10-22T09:44:19.658404700Z"
      },
      {
        "name": "WEATHER_DISPLAYER2",
        "metadata": {
          "type": "digital",
          "displayedData": [
            "wind",
            "humidity",
            "temperature"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9b"
          }
        ],
        "createdAt": "2024-10-22T09:44:19.662409200Z",
        "updatedAt": "2024-10-22T09:44:19.662409200Z"
      }
    ],
    "count": 2
  }
  ~~~

- **Update devices:** PUT /serviceregistry/mgmt/devices

  Request body:
  ~~~
  {
    "devices": [
      {
        "name": "WEATHER_DISPLAYER1",
        "metadata": {
          "type": "analogue",
          "displayedData": [ "temperature" ]
        },
        "addresses": [ "ab:f8:c8:e1:45:96" ]
      },
      {
        "name": "WEATHER_DISPLAYER2",
        "metadata": {
          "type": "digital",
          "displayedData": [ "wind", "temperature" ]
        },
        "addresses": [ "ab:ff:c8:e1:45:9b" ]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "WEATHER_DISPLAYER2",
        "metadata": {
          "type": "analogue",
          "displayedData": [
            "temperature"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:f8:c8:e1:45:96"
          }
        ],
        "createdAt": "2024-10-22T09:44:20Z",
        "updatedAt": "2024-10-22T14:41:00.839368100Z"
      },
      {
        "name": "WEATHER_DISPLAYER2",
        "metadata": {
          "type": "digital",
          "displayedData": [
            "wind",
            "temperature"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9b"
          }
        ],
        "createdAt": "2024-10-22T09:44:20Z",
        "updatedAt": "2024-10-22T14:41:00.839368100Z"
      }
    ],
    "count": 2
  }
  ~~~

- **Query devices:** POST /serviceregistry/mgmt/devices/query
  
  Request body:
  ~~~
  {
    "pagination": {
      "page": 1,
      "size": 2,
      "direction": "DESC",
      "sortField": "name"
    },
    "deviceNames": [
      "THERMOMETER1", "THERMOMETER2", "THERMOMETER3", "THERMOMETER4", "THERMOMETER5"
    ],
    "addresses": [
    ],
    "addressType": "",
    "metadataRequirementList": [
      {
        "appliable": { "op": "CONTAINS", "value": "spring" },
        "scales": { "op": "CONTAINS", "value": "Fahrenheit" },
        "maxTemperature.Fahrenheit": { "op": "GREATER_THAN", "value": 100 }
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "THERMOMETER3",
        "metadata": {
          "scales": [
            "Fahrenheit"
          ],
          "maxTemperature": {
            "Fahrenheit": 110
          },
          "minTemperature": {
            "Fahrenheit": 0
          },
          "appliable": [
            "winter",
            "spring",
            "autumn"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "81:ef:1a:44:7b:01"
          }
        ],
        "createdAt": "2024-10-22T10:30:55Z",
        "updatedAt": "2024-10-22T10:30:55Z"
      },
      {
        "name": "THERMOMETER2",
        "metadata": {
          "scales": [
            "Fahrenheit"
          ],
          "maxTemperature": {
            "Fahrenheit": 140
          },
          "minTemperature": {
            "Fahrenheit": 0
          },
          "appliable": [
            "winter",
            "spring",
            "summer",
            "autumn"
          ]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "81:ef:1a:44:7b:00"
          }
        ],
        "createdAt": "2024-10-22T10:01:19Z",
        "updatedAt": "2024-10-22T10:01:19Z"
      }
    ],
    "count": 4
  }
  ~~~

- **Delete devices:** DELETE /serviceregistry/mgmt/devices
  
  Query parameters: THERMOMETER2, THERMOMETER10

  ~~~
  http://localhost:8443/serviceregistry/mgmt/devices?names=THERMOMETER2&names=THERMOMETER10
  ~~~

  Response code: 200
  
## **System-related**

- **Create systems:** POST /serviceregistry/mgmt/systems

  Request body:
  ~~~
  {
    "systems": [
      {
        "name": "TestTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "scales": []
        },
        "version": "",
        "addresses": [ "ttp.greenhouse.com" ],
        "deviceName": ""
      },
      {
        "name": "MainTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": [ "Fahrenheit", "Kelvin", "Celsius" ]
        },
        "version": "2",
        "addresses": [ "192.168.1.2", "mtp.greenhouse.com" ],
        "deviceName": "THERMOMETER5"
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "TestTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "scales": []
        },
        "version": "1.0.0",
        "addresses": [
          {
            "type": "HOSTNAME",
            "address": "ttp.greenhouse.com"
          }
        ],
        "createdAt": "2024-10-23T21:43:34.863607800Z",
        "updatedAt": "2024-10-23T21:43:34.863607800Z"
      },
      {
        "name": "MainTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": [
            "Fahrenheit",
            "Kelvin",
            "Celsius"
          ]
        },
        "version": "2.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          },
          {
            "type": "HOSTNAME",
            "address": "mtp.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER5",
          "metadata": {
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ],
            "maxTemperature": {
              "Kelvin": 310,
              "Celsius": 40,
              "Fahrenheit": 140
            },
            "minTemperature": {
              "Fahrenheit": 20,
              "Kelvin": 260,
              "Celsius": -10
            },
            "appliable": [
              "spring",
              "summer",
              "autumn",
              "winter"
            ]
          },
          "addresses": [
            {
              "type": "MAC",
              "address": "81:ef:1a:44:7b:02"
            }
          ],
          "createdAt": "2024-10-22T10:34:10Z",
          "updatedAt": "2024-10-22T10:34:10Z"
        },
        "createdAt": "2024-10-23T21:43:34.869614800Z",
        "updatedAt": "2024-10-23T21:43:34.869614800Z"
      }
    ],
    "count": 2
  }
  ~~~

  - **Update systems:** PUT /serviceregistry/mgmt/systems
  
  Request body:
  ~~~
  {
    "systems": [
      {
        "name": "TemperatureConsumer",
        "metadata": {
          "type": "temperature",
          "screenSize": { "width": 620, "height": 350 }
        },
        "version": "2.2.1",
        "addresses": [ "tc.greenhouse.com" ],
        "deviceName": "WEATHER_DISPLAYER1"
      },
      {
        "name": "MainTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": [ "Fahrenheit", "Kelvin", "Celsius" ]
        },
        "version": "2.1",
        "addresses": [ "192.168.1.2", "mtp.greenhouse.com" ],
        "deviceName": "THERMOMETER5"
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "MainTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": [
            "Fahrenheit",
            "Kelvin",
            "Celsius"
          ]
        },
        "version": "2.1.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          },
          {
            "type": "HOSTNAME",
            "address": "mtp.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER5",
          "metadata": {
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ],
            "maxTemperature": {
              "Kelvin": 310,
              "Celsius": 40,
              "Fahrenheit": 140
            },
            "minTemperature": {
              "Fahrenheit": 20,
              "Kelvin": 260,
              "Celsius": -10
            },
            "appliable": [
              "spring",
              "summer",
              "autumn",
              "winter"
            ]
          },
          "addresses": [
            {
              "type": "MAC",
              "address": "81:ef:1a:44:7b:02"
            }
          ],
          "createdAt": "2024-10-22T10:34:10Z",
          "updatedAt": "2024-10-22T10:34:10Z"
        },
        "createdAt": "2024-11-03T20:47:36Z",
        "updatedAt": "2024-11-06T10:14:22.625107800Z"
      },
      {
        "name": "TemperatureConsumer",
        "metadata": {
          "type": "temperature",
          "screenSize": {
            "width": 620,
            "height": 350
          }
        },
        "version": "2.2.1",
        "addresses": [
          {
            "type": "HOSTNAME",
            "address": "tc.greenhouse.com"
          }
        ],
        "device": {
          "name": "WEATHER_DISPLAYER1",
          "metadata": {
            "type": "analogue",
            "displayedData": [
              "temperature"
            ]
          },
          "addresses": [
            {
              "type": "MAC",
              "address": "ab:f8:c8:e1:45:95"
            }
          ],
          "createdAt": "2024-10-22T09:44:20Z",
          "updatedAt": "2024-10-22T09:44:20Z"
        },
        "createdAt": "2024-10-23T21:53:08Z",
        "updatedAt": "2024-11-06T10:14:22.652566Z"
      }
    ],
    "count": 2
  }
  ~~~

  - **Query systems:** POST /serviceregistry/mgmt/systems/query
  
  _Query parameter: verbose=false_
  
  Request body:
  ~~~
  {
    "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "name"
    },
    "systemNames": [
    ],
    "addresses": [
    ],
    "addressType": "",
    "metadataRequirementList": [
      {
        "type": "temperature"
      }
    ],
    "versions": [ "2.0.0", "2.1.0", "2.2.1", "1.0.0" ],
    "deviceNames": [
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "MainTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": [
            "Fahrenheit",
            "Kelvin",
            "Celsius"
          ]
        },
        "version": "2.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.1.2"
          },
          {
            "type": "HOSTNAME",
            "address": "mtp.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER5"
        },
        "createdAt": "2024-10-23T21:43:35Z",
        "updatedAt": "2024-10-23T21:43:35Z"
      },
      {
        "name": "TemperatureConsumer",
        "metadata": {
          "type": "temperature",
          "screenSize": {
            "width": 600,
            "height": 350
          }
        },
        "version": "2.2.1",
        "addresses": [
          {
            "type": "HOSTNAME",
            "address": "tc.greenhouse.com"
          }
        ],
        "device": {
          "name": "WEATHER_DISPLAYER1"
        },
        "createdAt": "2024-10-23T21:53:08Z",
        "updatedAt": "2024-10-23T23:59:41Z"
      },
      {
        "name": "TemperatureProvider1",
        "metadata": {
          "type": "temperature",
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "customizable": false
        },
        "version": "2.1.0",
        "addresses": [
          {
            "type": "HOSTNAME",
            "address": "tp1.greenhouse.com"
          },
          {
            "type": "IPV4",
            "address": "192.168.66.1"
          }
        ],
        "device": {
          "name": "THERMOMETER1"
        },
        "createdAt": "2024-10-23T21:33:05Z",
        "updatedAt": "2024-10-23T21:33:05Z"
      },
      {
        "name": "TestTemperatureProvider",
        "metadata": {
          "type": "temperature",
          "scales": []
        },
        "version": "1.0.0",
        "addresses": [
          {
            "type": "HOSTNAME",
            "address": "ttp.greenhouse.com"
          }
        ],
        "createdAt": "2024-10-23T21:43:35Z",
        "updatedAt": "2024-10-23T21:43:35Z"
      }
    ],
    "count": 4
  }
  ~~~

  - **Delete systems:** DELETE /serviceregistry/mgmt/systems
  
  Query parameters: TestTemperatureConsumer1, TestTemperatureConsumer2

  ~~~
  http://localhost:8443/serviceregistry/mgmt/systems?names=TestTemperatureVonsumer1&names=TestTemperatureConsumer1
  ~~~

  Response code: 200

## **Service definition-related**

- **Create service definitions:** POST /serviceregistry/mgmt/service-definitions

  Request body:
  ~~~
  {
    "serviceDefinitionNames": [
      "fahrenheitInfo", "kelvinInfo", "celsiusInfo"
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "fahrenheitInfo",
        "createdAt": "2024-10-24T21:48:35.519522800Z",
        "updatedAt": "2024-10-24T21:48:35.519522800Z"
      },
      {
        "name": "kelvinInfo",
        "createdAt": "2024-10-24T21:48:35.553525200Z",
        "updatedAt": "2024-10-24T21:48:35.553525200Z"
      },
      {
        "name": "celsiusInfo",
        "createdAt": "2024-10-24T21:48:35.556578200Z",
        "updatedAt": "2024-10-24T21:48:35.556578200Z"
      }
    ],
    "count": 3
  }
  ~~~

- **Query service definitions:** POST /serviceregistry/mgmt/service-definitions/query

  Request body:
  ~~~
  {
    "page": 2,
    "size": 5,
    "direction": "ASC",
    "sortField": "id"
  }
  ~~~

  Respone body:
  ~~~
  {
    "entries": [
      {
        "name": "celsiusInfo",
        "createdAt": "2024-10-24T21:48:36Z",
        "updatedAt": "2024-10-24T21:48:36Z"
      }
    ],
    "count": 11
  }
  ~~~

- **Delete service definitions:** DELETE /serviceregistry/mgmt/service-definitions
  
  Query parameter: kelvinInfo, info

  ~~~
  http://localhost:8443/serviceregistry/mgmt/service-definitions?names=kelvinInfo&names=info
  ~~~

  Response code: 200

## **Service instance-related**

- **Create service instances:** POST /serviceregistry/mgmt/service-instances
  
  Request body:
  ~~~
  {
    "instances": [
      {
        "systemName": "MainTemperatureProvider",
        "serviceDefinitionName": "fahrenheitInfo",
        "version": "1",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 300
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [ "192.168.1.2" ],
              "accessPort": 4040,
              "operations": 
              {
                "discover": {"path": "/discover", "method": "GET"}, 
                "query": {"path": "/query", "method": "POST"}
              },
              "basePath": "/info/fahrenheit"
            }
          }
        ]
      },
      {
        "systemName": "MainTemperatureProvider",
        "serviceDefinitionName": "celsiusInfo",
        "version": "1",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 250
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [ "192.168.1.2" ],
              "accessPort": 4040,
              "basePath": "/info/celsius"
            }
          }
        ]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "instanceId": "MainTemperatureProvider|fahrenheitInfo|1.0.0",
        "provider": {
          "name": "MainTemperatureProvider",
          "metadata": {
            "type": "temperature",
            "customizable": true,
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ]
          },
          "version": "2.0.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            },
            {
              "type": "HOSTNAME",
              "address": "mtp.greenhouse.com"
            }
          ],
          "device": {
            "name": "THERMOMETER5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "maxTemperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "minTemperature": {
                "Fahrenheit": 20,
                "Kelvin": 260,
                "Celsius": -10
              },
              "appliable": [
                "spring",
                "summer",
                "autumn",
                "winter"
              ]
            },
            "addresses": [
              {
                "type": "MAC",
                "address": "81:ef:1a:44:7b:02"
              }
            ],
            "createdAt": "2024-10-22T10:34:10Z",
            "updatedAt": "2024-10-22T10:34:10Z"
          },
          "createdAt": "2024-10-23T21:43:35Z",
          "updatedAt": "2024-10-23T21:43:35Z"
        },
        "serviceDefinition": {
          "name": "fahrenheitInfo",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 300
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4040,
              "operations": {
                "discover": {
                  "path": "/discover",
                  "method": "GET"
                },
                "query": {
                  "path": "/query",
                  "method": "POST"
                }
              },
              "basePath": "/info/fahrenheit"
            }
          }
        ],
        "createdAt": "2024-10-24T23:07:00.630946400Z",
        "updatedAt": "2024-10-24T23:07:00.630946400Z"
      },
      {
        "instanceId": "MainTemperatureProvider|celsiusInfo|1.0.0",
        "provider": {
          "name": "MainZemperatureProvider",
          "metadata": {
            "type": "temperature",
            "customizable": true,
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ]
          },
          "version": "2.0.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            },
            {
              "type": "HOSTNAME",
              "address": "mtp.greenhouse.com"
            }
          ],
          "device": {
            "name": "THERMOMETER5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "maxTemperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "minTemperature": {
                "Fahrenheit": 20,
                "Kelvin": 260,
                "Celsius": -10
              },
              "appliable": [
                "spring",
                "summer",
                "autumn",
                "winter"
              ]
            },
            "addresses": [
              {
                "type": "MAC",
                "address": "81:ef:1a:44:7b:02"
              }
            ],
            "createdAt": "2024-10-22T10:34:10Z",
            "updatedAt": "2024-10-22T10:34:10Z"
          },
          "createdAt": "2024-10-23T21:43:35Z",
          "updatedAt": "2024-10-23T21:43:35Z"
        },
        "serviceDefinition": {
          "name": "celsiusInfo",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 250
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4040,
              "basePath": "/info/celsius"
            }
          }
        ],
        "createdAt": "2024-10-24T23:07:00.640972600Z",
        "updatedAt": "2024-10-24T23:07:00.640972600Z"
      }
    ],
    "count": 2
  }
  ~~~

- **Update service instances:** PUT /serviceregistry/mgmt/service-instances
  
  Request body:
  ~~~
  {
    "instances": [
      {
        "instanceId": "MainTemperatureProvider|celsiusInfo|1.0.0",
        "expiresAt": "2035-10-11T14:30:00Z",
        "metadata": {
          "frequency": 355
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [ "192.168.1.2" ],
              "accessPort": 4040,
              "operations": 
              {
                "discover": {"path": "/discover", "method": "GET"}, 
                "query": {"path": "/query", "method": "POST"}
              },
              "basePath": "/info/fahrenheit"
            }
          },
          {
            "templateName": "generic_https",
            "protocol": "https",
            "policy": "NONE",
            "properties": {
              "accessAddresses": ["192.168.1.2"],
              "accessPort": 4041,
              "operations": 
              {
                "discover": {"path": "/discover", "method": "GET"}, 
                "query": {"path": "/query", "method": "POST"}
              },
              "basePath": "/info/celsius"
            }
          }
        ]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "instanceId": "MainTemperatureProvider|celsiusInfo|1.0.0",
        "provider": {
          "name": "MainTemperatureProvider",
          "metadata": {
            "type": "temperature",
            "customizable": true,
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ]
          },
          "version": "2.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            },
            {
              "type": "HOSTNAME",
              "address": "mtp.greenhouse.com"
            }
          ],
          "device": {
            "name": "THERMOMETER5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "maxTemperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "minTemperature": {
                "Fahrenheit": 20,
                "Kelvin": 260,
                "Celsius": -10
              },
              "appliable": [
                "spring",
                "summer",
                "autumn",
                "winter"
              ]
            },
            "addresses": [
              {
                "type": "MAC",
                "address": "81:ef:1a:44:7b:02"
              }
            ],
            "createdAt": "2024-10-22T10:34:10Z",
            "updatedAt": "2024-10-22T10:34:10Z"
          },
          "createdAt": "2024-11-03T20:47:36Z",
          "updatedAt": "2024-11-06T11:14:22Z"
        },
        "serviceDefinition": {
          "name": "celsiusInfo",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2035-10-11T14:30:00Z",
        "metadata": {
          "frequency": 355
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4040,
              "operations": {
                "discover": {
                  "path": "/discover",
                  "method": "GET"
                },
                "query": {
                  "path": "/query",
                  "method": "POST"
                }
              },
              "basePath": "/info/fahrenheit"
            }
          },
          {
            "templateName": "generic_https",
            "protocol": "https",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4041,
              "operations": {
                "discover": {
                  "path": "/discover",
                  "method": "GET"
                },
                "query": {
                  "path": "/query",
                  "method": "POST"
                }
             },
              "basePath": "/info/celsius"
            }
          }
        ],
        "createdAt": "2024-11-06T10:35:24Z",
        "updatedAt": "2024-11-06T10:36:03.441349900Z"
      }
    ],
    "count": 1
  }
  ~~~

- **Query service instances:** POST /serviceregistry/mgmt/service-instances/query

  _Query parameter: verbose=true_
  
  Request body:
  ~~~
  {
   "pagination": {
      "page": 0,
      "size": 10,
      "direction": "DESC",
      "sortField": "id"
    },
    "instanceIds": [
    ],
    "providerNames": [
      "TemperatureProvider1", "MainTemperatureProvider"
    ],
    "serviceDefinitionNames": [
    ],
    "versions": [
    ],
    "alivesAt": "",
    "metadataRequirementsList": [
      {
        "frequency": { "op": "LESS_THAN", "value": 500 }
      }
    ],
    "interfaceTemplateNames": [
      "generic_http", "generic_https"
    ],
    "interfacePropertyRequirementsList": [
      {
        "accessAddresses": [ "192.168.1.2" ],
        "accessPort": 4040,
        "basePath": "/info/fahrenheit"
      },
      {
        "accessAddresses": [ "192.168.1.2" ],
        "accessPort": 4040,
        "basePath": "/info/celsius"
      },
      {
        "accessAddresses": [ "192.168.66.1" ],
        "accessPort": 4040,
        "basePath": "/celsius-info"
      }
    ],
    "policies": [
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "instanceId": "MainTemperatureProvider|celsiusInfo|1.0.0",
        "provider": {
          "name": "MainTemperatureProvider",
          "metadata": {
            "type": "temperature",
            "customizable": true,
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ]
          },
          "version": "2.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            },
            {
              "type": "HOSTNAME",
              "address": "mtp.greenhouse.com"
            }
          ],
          "device": {
            "name": "THERMOMETER5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "maxTemperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "minTemperature": {
                "Fahrenheit": 20,
                "Kelvin": 260,
                "Celsius": -10
              },
              "appliable": [
                "spring",
                "summer",
                "autumn",
                "winter"
              ]
            },
            "addresses": [
              {
                "type": "MAC",
                "address": "81:ef:1a:44:7b:02"
              }
            ],
            "createdAt": "2024-10-22T10:34:10Z",
            "updatedAt": "2024-10-22T10:34:10Z"
          },
          "createdAt": "2024-11-03T20:47:36Z",
          "updatedAt": "2024-11-06T11:14:22Z"
        },
        "serviceDefinition": {
          "name": "celsius-info",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2035-10-11T14:30:00Z",
        "metadata": {
          "frequency": 355
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4040,
              "operations": {
                "discover": {
                  "path": "/discover",
                  "method": "GET"
                },
                "query": {
                  "path": "/query",
                  "method": "POST"
                }
              },
              "basePath": "/info/celsius"
            }
          },
          {
            "templateName": "generic_https",
            "protocol": "https",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4041,
              "operations": {
                "discover": {
                  "path": "/discover",
                  "method": "GET"
                },
                "query": {
                  "path": "/query",
                  "method": "POST"
                }
              },
              "basePath": "/info/celsuis"
            }
          }
        ],
        "createdAt": "2024-11-06T10:35:24Z",
        "updatedAt": "2024-11-06T11:36:03Z"
      },
      {
        "instanceId": "MainTemperatureProvider|fahrenheitInfo|1.0.0",
        "provider": {
          "name": "MainTemperatureProvider",
          "metadata": {
            "type": "temperature",
            "customizable": true,
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ]
          },
          "version": "2.1.0",
          "addresses": [
            {
              "type": "IPV4",
              "address": "192.168.1.2"
            },
            {
              "type": "HOSTNAME",
              "address": "mtp.greenhouse.com"
            }
          ],
          "device": {
            "name": "THERMOMETER",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "maxTemperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "minTemperature": {
                "Fahrenheit": 20,
                "Kelvin": 260,
                "Celsius": -10
              },
              "appliable": [
                "spring",
                "summer",
                "autumn",
                "winter"
              ]
            },
            "addresses": [
              {
                "type": "MAC",
                "address": "81:ef:1a:44:7b:02"
              }
            ],
            "createdAt": "2024-10-22T10:34:10Z",
            "updatedAt": "2024-10-22T10:34:10Z"
          },
          "createdAt": "2024-11-03T20:47:36Z",
          "updatedAt": "2024-11-06T11:14:22Z"
        },
        "serviceDefinition": {
          "name": "fahrenheit-info",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 300
        },
        "interfaces": [
          {
            "templateName": "generic_http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.1.2"
              ],
              "accessPort": 4040,
              "operations": {
                "discover": {
                  "path": "/discover",
                  "method": "GET"
                },
                "query": {
                  "path": "/query",
                  "method": "POST"
                }
              },
              "basePath": "/info/fahrenheit"
            }
          }
        ],
        "createdAt": "2024-11-06T10:35:24Z",
        "updatedAt": "2024-11-06T10:35:24Z"
      },
      {
        "instanceId": "TemperatureProvider1|celsiusInfo|1.0.0",
        "provider": {
          "name": "TemperatureProvider1",
          "metadata": {
            "type": "temperature",
            "scales": [
              "Kelvin",
              "Celsius"
            ],
            "customizable": false
          },
          "version": "2.1.0",
          "addresses": [
            {
              "type": "HOSTNAME",
              "address": "tp1.greenhouse.com"
            },
            {
              "type": "IPV4",
              "address": "192.168.66.1"
            }
          ],
          "device": {
            "name": "THERMOMETER1",
            "metadata": {
              "scales": [
                "Kelvin",
                "Celsius"
              ],
              "maxTemperature": {
                "Kelvin": 310,
                "Celsius": 40
              },
              "minTemperature": {
                "Kelvin": 260,
                "Celsius": -10
              },
              "appliable": [
                "spring",
                "autumn"
              ]
            },
            "addresses": [
              {
                "type": "MAC",
                "address": "81:ef:1a:44:7a:ff"
              }
            ],
            "createdAt": "2024-10-21T18:47:45Z",
            "updatedAt": "2024-10-21T18:47:45Z"
          },
          "createdAt": "2024-11-03T20:20:50Z",
          "updatedAt": "2024-11-03T20:20:50Z"
        },
        "serviceDefinition": {
          "name": "celsius-info",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "unrestricted-discovery": true,
          "frequency": 400
        },
        "interfaces": [
          {
            "templateName": "generic_https",
            "protocol": "https",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.66.1"
              ],
              "accessPort": 4040,
              "operations": {
                "query-temperature": {
                  "path": "/celsius-info",
                  "method": "GET"
                }
              },
              "basePath": "/celsius-info"
            }
          }
        ],
        "createdAt": "2024-11-03T20:21:05Z",
        "updatedAt": "2024-11-03T21:28:29Z"
      }
    ],
    "count": 3
  }
  ~~~

- **Delete service instances:** DELETE /serviceregistry/mgmt/service-instances
  
  Query parameters: MainTemperatureProvider|celsiusInfo|1.0.0, MainTemperatureProvider|fahrenheitInfo|1.0.0

  ~~~
  http://localhost:8443/serviceregistry/mgmt/service-instances?serviceInstances=MainTemperatureProvider%7CcelsiusInfo%7C1.0.0&serviceInstances=MainTemperatureProvider%7CahrenheitInfo%7C1.0.0
  ~~~

Response code: 200

## **Interface template related**

- **Create interface templates:** POST /serviceregistry/mgmt/interface-templates
  
  Request body:
  ~~~
  {
    "interfaceTemplates": [
      {
        "name": "generic_mqtt",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          },
          {
            "name": "accessPort",
            "mandatory": true,
            "validator": "MINMAX",
            "validatorParams": [
              "1",
              "65535"
            ]
          },
          {
            "name": "topic",
            "mandatory": true
          },
          {
            "name": "clientId",
            "mandatory": false
          }
        ]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "generic_mqtt",
        "protocol": "tcp",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          },
          {
            "name": "accessPort",
            "mandatory": true,
            "validator": "MINMAX",
            "validatorParams": [
              "1",
              "65535"
            ]
          },
          {
            "name": "topic",
            "mandatory": true
          },
          {
            "name": "clientId",
            "mandatory": false
          }
        ],
        "createdAt": "2024-10-25T13:34:45.241134500Z",
        "updatedAt": "2024-10-25T13:34:45.241134500Z"
      }
    ],
    "count": 1
  }
  ~~~

- **Query interface templates:** POST /serviceregistry/mgmt/interface-templates/query

  Request body:
  ~~~
  {
    "pagination": {
      "page": 0,
      "size": 2,
      "direction": "DESC",
      "sortField": "id"
    },
    "templateNames": [
      "generic_http", "generic_https"
    ],
    "protocols": [
      "http", "https"
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "generic_https",
        "protocol": "https",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          },
          {
            "name": "accessPort",
            "mandatory": true,
            "validator": "PORT",
            "validatorParams": []
          },
          {
            "name": "basePath",
            "mandatory": true
          },
          {
            "name": "operations",
            "mandatory": false,
            "validator": "HTTP_OPERATIONS",
            "validatorParams": []
          }
        ],
        "createdAt": "2024-07-10T11:57:12Z",
        "updatedAt": "2024-10-15T18:15:44Z"
      },
      {
        "name": "generic_http",
        "protocol": "http",
        "propertyRequirements": [
          {
            "name": "accessAddresses",
            "mandatory": true,
            "validator": "NOT_EMPTY_ADDRESS_LIST",
            "validatorParams": []
          },
          {
            "name": "accessPort",
            "mandatory": true,
            "validator": "PORT",
            "validatorParams": []
          },
          {
            "name": "basePath",
            "mandatory": true
          },
          {
            "name": "operations",
            "mandatory": false,
            "validator": "HTTP_OPERATIONS",
            "validatorParams": []
          }
        ],
        "createdAt": "2024-07-10T11:57:12Z",
        "updatedAt": "2024-10-13T22:48:03Z"
      }
    ],
    "count": 2
  }
  ~~~

- **Delete interface templates:** DELETE /serviceregistry/mgmt/interface-templates
  
  Query parameters: generic_mqtt, test_template

  ~~~
  http://localhost:8443/serviceregistry/mgmt/interface-templates?names=generic_mqtt&names=test_template
  ~~~

  Response code: 200
  
# Example for an error response

If the request payload is not syntactically or semantically correct, an error message data tranfer object is returned.

**Here is an example for that:** 

DELETE /serviceregistry/service-discovery/revoke/{instanceId}

 ~~~
  http://localhost:8443/serviceregistry/service-discovery/revoke/TemperatureProvider1%7CcelsiusInfo%7C1.0.0
  ~~~

If the instanceId belongs to an other system's service, the sender will receive the following response:

  ~~~
  {
    "errorMessage": "Revoking other systems' service is forbidden",
    "errorCode": 403,
    "exceptionType": "FORBIDDEN",
    "origin": "DELETE /serviceregistry/service-discovery/revoke/TemperatureProvider1|celsiusInfo|1.0.0"
  }
  ~~~