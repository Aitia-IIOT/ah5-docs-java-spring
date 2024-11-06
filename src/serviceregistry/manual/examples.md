# **Examples of data transfer objects sent and received**

This file contains specific examples of what payloads are expected and returned by each endpoint maintained by the service registry.

# Discovery endpoints

## **Device discovery**
- **Register device:** POST /serviceregistry/device-discovery/register
  
  Request body: 
  ~~~
  {
    "name": "thermometer1",
    "metadata": {
      "scales": ["Kelvin", "Celsius"],
      "max-temperature": {"Kelvin": 310, "Celsius": 40},
      "min-temperature": {"Kelvin": 260, "Celsius": -10},
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
    "name": "thermometer1",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "max-temperature": {
        "Kelvin": 310,
        "Celsius": 40
      },
      "min-temperature": {
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
        "name": "thermometer1",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "max-temperature": {
            "Kelvin": 310,
            "Celsius": 40
          },
          "min-temperature": {
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
        "name": "weather-displayer1",
        "metadata": {
          "type": "analogue",
          "displayed-data": [
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
  
  Path parameter: test-weather-displayer
  ~~~
  http://localhost:8443/serviceregistry/device-discovery/revoke/test-weather-displayer
  ~~~

  Response code: 200 

##  **System discovery**

**Authentication:** For system related operations, the payload does not contain the system name, because it comes either from an authorization header or an X.509 certificate. 

In the following examples, self-declared authentication is used, so the header should contain the system name with the  _SYSTEM//_ prefix. The system name will be _temperature-provider1_ in the examples. This should be changed to the name of the actual system.

  ~~~
  -H 'Authorization: Bearer SYSTEM//temperature-provider1' \
  ~~~

The header is also included in the examples where the system name comes from it.

- **Register system:** POST /serviceregistry/system-discovery/register

  Header:
  ~~~
  curl -X 'POST' \
    'http://localhost:8443/serviceregistry/system-discovery/register' \
    -H 'accept: application/json' \
    -H 'Authorization: Bearer SYSTEM//temperature-provider1' \
    -H 'Content-Type: application/json' \
  ~~~
  Request body:
  ~~~
  {
    "metadata": {
      "type": "temperature",
      "scales": ["Kelvin", "Celsius"],
      "customizable": false
    },
    "version": "2.1",
    "addresses": ["tp1.greenhouse.com", "192.168.66.1"],
    "deviceName": "thermometer1"
  }
  ~~~

  Response body:
  ~~~
  {
    "name": "temperature-provider1",
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
      "name": "thermometer1",
      "metadata": {
        "scales": [
          "Kelvin",
          "Celsius"
        ],
        "max-temperature": {
          "Kelvin": 310,
          "Celsius": 40
        },
        "min-temperature": {
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
      "thermometer1", "thermometer2", "thermometer3", "thermometer5"
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "main-temperature-provider",
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
          "name": "thermometer5",
          "metadata": {
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ],
            "max-temperature": {
              "Kelvin": 310,
              "Celsius": 40,
              "Fahrenheit": 140
            },
            "min-temperature": {
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
  -H 'Authorization: Bearer SYSTEM//temperature-provider1'
~~~

~~~
http://localhost:8443/serviceregistry/system-discovery/revoke
~~~

Reponse code: 200 

## **Service discovery**

**Authentication:** For service related operations, the payload does not contain the system name, because it comes either from an authorization header or an X.509 certificate. 

The Service Registry generates the Service Instance ID from the system name. It also uses system name to identify who sent the request.

In the following examples, self-declared authentication is used, so the header should contain the system name with the  _SYSTEM//_ prefix. The system name will be _temperature-provider1_ in the examples.

  ~~~
  -H 'Authorization: Bearer SYSTEM//temperature-provider1' \
  ~~~

- **Register service:** POST /serviceregistry/service-discovery/register
  
  Request body:
  ~~~
  {
    "serviceDefinitionName": "celsius-info",
    "version": "1",
    "expiresAt": "2030-10-11T14:30:00Z",
    "metadata": {
      "frequency": 400
    },
    "interfaces": [
      {
        "templateName": "generic-https",
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
    "instanceId": "temperature-provider1::celsius-info::1.0.0",
    "provider": {
      "name": "temperature-provider1",
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
        "name": "thermometer1",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "max-temperature": {
            "Kelvin": 310,
            "Celsius": 40
          },
          "min-temperature": {
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
      "name": "celsius-info",
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
        "templateName": "generic-https",
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
        "temperature-provider1" 
    ],
    "serviceDefinitionNames": [
        "celsius-info"
    ],
    "versions": [
        "1.0.0", "1.0.1"
    ],
    "alivesAt": "",
    "metadataRequirementsList": [
    ],
    "interfaceTemplateNames": [
        "generic-http", "generic-https"
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
        "instanceId": "temperature-provider1::celsius-info::1.0.0",
        "provider": {
          "name": "temperature-provider1",
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
            "templateName": "generic-https",
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
  
  Path parameter: temperature-provider1::celsius-info::1.1.0

  ~~~
  http://localhost:8443/serviceregistry/service-discovery/revoke/temperature-provider1%3A%3Acelsius-info%3A%3A1.1.0
  ~~~

  Response code: 200

## **Logs**
/serviceregistry/mgmt/logs

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
GET 
/serviceregistry/mgmt/get-config

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

## **Device related**

- **Create devices:** POST /serviceregistry/mgmt/devices
  
  Request body:
  ~~~
  {
    "devices": [
      {
        "name": "weather-displayer1",
        "metadata": {
          "type": "analogue",
          "displayed-data": ["temperature"]
        },
        "addresses": ["ab:ff:c8:e1:45:9a"]
      },
      {
        "name": "weather-displayer2",
        "metadata": {
          "type": "digital",
          "displayed-data": ["wind", "humidity", "temperature"]
        },
        "addresses": ["ab:ff:c8:e1:45:9b"]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "weather-displayer1",
        "metadata": {
          "type": "analogue",
          "displayed-data": [
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
        "name": "weather-displayer2",
        "metadata": {
          "type": "digital",
          "displayed-data": [
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
        "name": "weather-displayer1",
        "metadata": {
          "type": "analogue",
          "displayed-data": ["temperature"]
        },
        "addresses": ["ab:f8:c8:e1:45:96"]
      },
      {
        "name": "weather-displayer2",
        "metadata": {
          "type": "digital",
          "displayed-data": ["wind", "temperature"]
        },
        "addresses": ["ab:ff:c8:e1:45:9b"]
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "weather-displayer1",
        "metadata": {
          "type": "analogue",
          "displayed-data": [
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
        "name": "weather-displayer2",
        "metadata": {
          "type": "digital",
          "displayed-data": [
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
      "thermometer1", "thermometer2", "thermometer3", "thermometer4", "thermometer5"
    ],
    "addresses": [
    ],
    "addressType": "",
    "metadataRequirementList": [
      {
        "appliable": { "op": "CONTAINS", "value": "spring"},
        "scales": { "op": "CONTAINS", "value": "Fahrenheit" },
        "max-temperature.Fahrenheit": { "op": "GREATER_THAN", "value": 100}
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "thermometer3",
        "metadata": {
          "scales": [
            "Fahrenheit"
          ],
          "max-temperature": {
            "Fahrenheit": 110
          },
          "min-temperature": {
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
        "name": "thermometer2",
        "metadata": {
          "scales": [
            "Fahrenheit"
          ],
          "max-temperature": {
            "Fahrenheit": 140
          },
          "min-temperature": {
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
  
  Query parameters: thermometer2, thermometer10

  ~~~
  http://localhost:8443/serviceregistry/mgmt/devices?names=thermometer2&names=thermometer10
  ~~~

  Response code: 200
  
## **System related**

- **Create systems:** POST 
/serviceregistry/mgmt/systems

  Request body:
  ~~~
  {
    "systems": [
      {
        "name": "test-temperature-provider",
        "metadata": {
          "type": "temperature",
          "scales": []
        },
        "version": "",
        "addresses": ["ttp.greenhouse.com"],
        "deviceName": ""
      },
      {
        "name": "main-temperature-provider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": ["Fahrenheit", "Kelvin", "Celsius"]
        },
        "version": "2",
        "addresses": ["192.168.1.2", "mtp.greenhouse.com"],
        "deviceName": "thermometer5"
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "test-temperature-provider",
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
        "name": "main-temperature-provider",
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
          "name": "thermometer5",
          "metadata": {
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ],
            "max-temperature": {
              "Kelvin": 310,
              "Celsius": 40,
              "Fahrenheit": 140
            },
            "min-temperature": {
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
        "name": "temperature-consumer",
        "metadata": {
          "type": "temperature",
          "screen-size": {"width": 620, "height": 350}
        },
        "version": "2.2.1",
        "addresses": ["tc.greenhouse.com"],
        "deviceName": "weather-displayer1"
      },
      {
        "name": "main-temperature-provider",
        "metadata": {
          "type": "temperature",
          "customizable": true,
          "scales": ["Fahrenheit", "Kelvin", "Celsius"]
        },
        "version": "2.1",
        "addresses": ["192.168.1.2", "mtp.greenhouse.com"],
        "deviceName": "thermometer5"
      }
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "main-temperature-provider",
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
          "name": "thermometer5",
          "metadata": {
            "scales": [
              "Fahrenheit",
              "Kelvin",
              "Celsius"
            ],
            "max-temperature": {
              "Kelvin": 310,
              "Celsius": 40,
              "Fahrenheit": 140
            },
            "min-temperature": {
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
        "name": "temperature-consumer",
        "metadata": {
          "type": "temperature",
          "screen-size": {
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
          "name": "weather-displayer1",
          "metadata": {
            "type": "analogue",
            "displayed-data": [
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
    "versions": ["2.0.0", "2.1.0", "2.2.1", "1.0.0"],
    "deviceNames": [
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "main-temperature-provider",
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
          "name": "thermometer5"
        },
        "createdAt": "2024-10-23T21:43:35Z",
        "updatedAt": "2024-10-23T21:43:35Z"
      },
      {
        "name": "temperature-consumer",
        "metadata": {
          "type": "temperature",
          "screen-size": {
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
          "name": "weather-displayer1"
        },
        "createdAt": "2024-10-23T21:53:08Z",
        "updatedAt": "2024-10-23T23:59:41Z"
      },
      {
        "name": "temperature-provider1",
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
          "name": "thermometer1"
        },
        "createdAt": "2024-10-23T21:33:05Z",
        "updatedAt": "2024-10-23T21:33:05Z"
      },
      {
        "name": "test-temperature-provider",
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
  
  Query parameters: test-temperature-consumer-1, test-temperature-consumer-2

  ~~~
  http://localhost:8443/serviceregistry/mgmt/systems?names=test-temperature-consumer-1&names=test-temperature-consumer-1
  ~~~

  Response code: 200

## **Service definition related**

- **Create service definitions:** POST /serviceregistry/mgmt/service-definitions

  Request body:
  ~~~
  {
    "serviceDefinitionNames": [
      "Fahrenheit-info", "Kelvin-info", "Celsius-info"
    ]
  }
  ~~~

  Response body:
  ~~~
  {
    "entries": [
      {
        "name": "fahrenheit-info",
        "createdAt": "2024-10-24T21:48:35.519522800Z",
        "updatedAt": "2024-10-24T21:48:35.519522800Z"
      },
      {
        "name": "kelvin-info",
        "createdAt": "2024-10-24T21:48:35.553525200Z",
        "updatedAt": "2024-10-24T21:48:35.553525200Z"
      },
      {
        "name": "celsius-info",
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
        "name": "celsius-info",
        "createdAt": "2024-10-24T21:48:36Z",
        "updatedAt": "2024-10-24T21:48:36Z"
      }
    ],
    "count": 11
  }
  ~~~

- **Delete service definitions:** DELETE /serviceregistry/mgmt/service-definitions
  
  Query parameter: Kelvin-info, info

  ~~~
  http://localhost:8443/serviceregistry/mgmt/service-definitions?names=Kelvin-info&names=info
  ~~~

  Response code: 200

## **Service instance related**

- **Create service instances:** POST /serviceregistry/mgmt/service-instances
  
  Request body:
  ~~~
  {
    "instances": [
      {
        "systemName": "main-temperature-provider",
        "serviceDefinitionName": "fahrenheit-info",
        "version": "1",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 300
        },
        "interfaces": [
          {
            "templateName": "generic-http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": ["192.168.1.2"],
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
        "systemName": "main-temperature-provider",
        "serviceDefinitionName": "celsius-info",
        "version": "1",
        "expiresAt": "2030-10-11T14:30:00Z",
        "metadata": {
          "frequency": 250
        },
        "interfaces": [
          {
            "templateName": "generic-http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": ["192.168.1.2"],
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
        "instanceId": "main-temperature-provider::fahrenheit-info::1.0.0",
        "provider": {
          "name": "main-temperature-provider",
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
            "name": "thermometer5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "max-temperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "min-temperature": {
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
            "templateName": "generic-http",
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
        "instanceId": "main-temperature-provider::celsius-info::1.0.0",
        "provider": {
          "name": "main-temperature-provider",
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
            "name": "thermometer5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "max-temperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "min-temperature": {
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
          "name": "celsius-info",
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
            "templateName": "generic-http",
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
        "instanceId": "main-temperature-provider::celsius-info::1.0.0",
        "expiresAt": "2035-10-11T14:30:00Z",
        "metadata": {
          "frequency": 355
        },
        "interfaces": [
          {
            "templateName": "generic-http",
            "protocol": "http",
            "policy": "NONE",
            "properties": {
              "accessAddresses": ["192.168.1.2"],
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
            "templateName": "generic-https",
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
        "instanceId": "main-temperature-provider::celsius-info::1.0.0",
        "provider": {
          "name": "main-temperature-provider",
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
            "name": "thermometer5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "max-temperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "min-temperature": {
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
            "templateName": "generic-http",
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
            "templateName": "generic-https",
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
      "temperature-provider1", "main-temperature-provider"
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
      "generic-http", "generic-https"
    ],
    "interfacePropertyRequirementsList": [
      {
        "accessAddresses": ["192.168.1.2"],
        "accessPort": 4040,
        "basePath": "/info/fahrenheit"
      },
      {
         "accessAddresses" : [ "192.168.66.1" ],
         "basePath" : "/celsius-info",
         "accessPort" : 4040
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
        "instanceId": "temperature-provider1::celsius-info::1.0.0",
        "provider": {
          "name": "temperature-provider1",
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
            "name": "thermometer1",
            "metadata": {
              "scales": [
                "Kelvin",
                "Celsius"
              ],
              "max-temperature": {
                "Kelvin": 310,
                "Celsius": 40
              },
              "min-temperature": {
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
          "name": "celsius-info",
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
            "templateName": "generic-https",
            "protocol": "https",
            "policy": "NONE",
            "properties": {
              "accessAddresses": [
                "192.168.66.1"
              ],
              "basePath": "/celsius-info",
              "accessPort": 4040
            }
          }
        ],
        "createdAt": "2024-10-24T23:47:51Z",
        "updatedAt": "2024-10-24T23:47:51Z"
      },
      {
        "instanceId": "main-temperature-provider::celsius-info::1.0.0",
        "provider": {
          "name": "main-temperature-provider",
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
            "name": "thermometer5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "max-temperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "min-temperature": {
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
          "name": "celsius-info",
          "createdAt": "2024-10-24T21:48:36Z",
          "updatedAt": "2024-10-24T21:48:36Z"
        },
        "version": "1.0.0",
        "expiresAt": "2035-10-11T14:30:00Z",
        "metadata": {
          "frequency": 350
        },
        "interfaces": [
          {
            "templateName": "generic-http",
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
            "templateName": "generic-https",
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
        "createdAt": "2024-10-24T23:07:01Z",
        "updatedAt": "2024-10-25T01:33:15Z"
      },
      {
        "instanceId": "main-temperature-provider::fahrenheit-info::1.0.0",
        "provider": {
          "name": "main-temperature-provider",
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
            "name": "thermometer5",
            "metadata": {
              "scales": [
                "Fahrenheit",
                "Kelvin",
                "Celsius"
              ],
              "max-temperature": {
                "Kelvin": 310,
                "Celsius": 40,
                "Fahrenheit": 140
              },
              "min-temperature": {
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
            "templateName": "generic-http",
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
          }
        ],
        "createdAt": "2024-10-24T23:07:01Z",
        "updatedAt": "2024-10-24T23:07:01Z"
      }
    ],
    "count": 3
  }
  ~~~

- **Delete service instances:** DELETE /serviceregistry/mgmt/service-instances
  
  Query parameters: main-temperature-provider::celsius-info::1.0.0, main-temperature-provider::fahrenheit-info::1.0.0

  ~~~
  http://localhost:8443/serviceregistry/mgmt/service-instances?serviceInstances=main-temperature-provider%3A%3Acelsius-info%3A%3A1.0.0&serviceInstances=main-temperature-provider%3A%3Afahrenheit-info%3A%3A1.0.0
  ~~~

Response code: 200

## **Interface template related**

- **Create interface templates:** POST /serviceregistry/mgmt/interface-templates
  
  Request body:
  ~~~
  {
    "interfaceTemplates": [
      {
        "name": "generic-mqtt",
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
        "name": "generic-mqtt",
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

- **Query interface templates:** POST 
/serviceregistry/mgmt/interface-templates/query

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
      "generic-http", "generic-https"
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
        "name": "generic-https",
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
        "name": "generic-http",
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
  
  Query parameters: generic-mqtt, test-template

  ~~~
  http://localhost:8443/serviceregistry/mgmt/interface-templates?names=generic-mqtt&names=test-template
  ~~~

  Response code: 200
  
# Example for an error response

If the request payload is not syntactically or semantically correct, an error message data tranfer object is returned.

**Here is an example for that:** 

DELETE /serviceregistry/service-discovery/revoke/{instanceId}

If the instanceId belongs to an other system's service, the sender will receive the following response:

  ~~~
  {
    "errorMessage": "Revoking other systems' service is forbidden",
    "errorCode": 403,
    "exceptionType": "FORBIDDEN",
    "origin": "DELETE /serviceregistry/service-discovery/revoke/{instanceId}"
  }
  ~~~