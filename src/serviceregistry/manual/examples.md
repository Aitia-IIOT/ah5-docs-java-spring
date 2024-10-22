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
      {
        "type": "MAC",
        "address": "81:ef:1a:44:7a:ff"
      }
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
  
  Query parameter: test-weather-displayer
  ~~~
  http://localhost:8443/serviceregistry/device-discovery/revoke/test-weather-displayer
  ~~~

  Response code: 200 
##  **System discovery**
## **Service discovery**

# Management endpoints

## **Device related**

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
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9a"
          }
        ]
      },
      {
        "name": "weather-displayer2",
        "metadata": {
          "type": "digital",
          "displayed-data": ["wind", "humidity", "temperature"]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9b"
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
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:f8:c8:e1:45:95"
          }
        ]
      },
      {
        "name": "weather-displayer2",
        "metadata": {
          "type": "digital",
          "displayed-data": ["wind", "temperature"]
        },
        "addresses": [
          {
            "type": "MAC",
            "address": "ab:ff:c8:e1:45:9b"
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

- **Delete devices:** DELETE /serviceregistry/mgmt/devices
  
  Query parameters: thermometer2, thermometer10

  ~~~
  http://localhost:8443/serviceregistry/mgmt/devices?names=thermometer2&names=thermometer10
  ~~~

  Response code: 200
  
## **System related**
## **Service definition related**
## **Service instance related**
## **Interface template related**
## **Configurations**
## **Logs**