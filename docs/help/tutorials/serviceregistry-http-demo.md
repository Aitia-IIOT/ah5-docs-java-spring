# ServiceRegistry HTTP Demo

## Recap

In this demo, we will demonstrate through a concrete example how a system should use the ServiceRegistry endpoints.

![image of the greenhouse](../../assets/pics/greenhouse.jpg)

Let's say, we have a greenhouse. Inside the greenhouse, there are plantations divided into blocks, and close to the blocks, _THERMOMETHERs_ are placed. We can monitor the temperature of the blocks, using the _Systems_ that run on the _THERMOMETERs_. If the temperature is too extreme, the _Systems_ may send a warning to our office.

There are two types of _THERMOMETERs_: indoor and outdoor _THERMOMETERs_, so we can even monitor the temperature outside the greenhouse.

![image of the office](../../assets/pics/office.jpg)

In our office, there are different _WEATHER\_DISPLAYERs_ that can show various data about the weather, such as wind or temperature. The temperature data measured by the _THERMOMETERs_ in the greenhouse is sent to these _WEATHER\_DISPLAYERs_. We also have _ALARMs_, this way we are immediately informed about extreme temperatures.

_THERMOMETERs_ can provide information on temperature in Celsius, Fahrenheit or Kelvin scales, but not all _THERMOMETERs_ are capable of sending data at all scales.

The concrete entities used in our example can be mapped to ServiceRegistry entities as follows:

![image of entities](../../assets/pics/entities.jpg)

- **Devices:** _THERMOMETERs_, _WEATHER\_DISPLAYERs_ and _ALARMs_
- **Systems:** _TemperatureProviders_ running on the THERMOMETERs, _TemperatureConsumers_ running on the WEATHER\_DISPLAYERs, and _AlertConsumers_ running on the ALARMs.
- **Services:** _fahrenheitInfo_, _kelvinInfo_, _celsiusInfo_, and _alertService_, provided by the TemperatureProviders and consumed by the TemperatureConsumers and the AlertConsumers

We will demonstrate the usage of the endpoints via two examples: [example 1](#example-1-provider) is about a provider system, and [example 2](#example-2-consumer) is about a consumer system.

## Example 1: Provider

In the following, we'll see how:

- a **system** called _TemperatureProvider2_, 
- running on the **device** named _THERMOMETER2_,
- publishes its **services:** _kelvinInfo_, _celsiusInfo_ and _alertService_

after registering itself into the Local Cloud.

### Step 1: Authentication

First of all, the system should perform some kind of [authentication](../../api/authentication_policy.md). In this example the [declared](../../api/authentication_policy.md/#declared) authentication strategy will be used.

To perform this type of authentication, the system must provide an authentication header for each request. This should consist of the _SYSTEM//_ prefix, followed by the name of the system. 

In our example, the authorization header will look like this:
~~~
-H 'Authorization: Bearer SYSTEM//TemperatureProvider2'
~~~

### Step 2: Lookup Devices

During registration, the system can specify which device it is running on. In this case, our device is called _THERMOMETER2_. Specifying the device is optional, but let's say, that in our example it is relevant to the operation of _TemperatureProvider2_. Therefore, the system first looks up if there is a device with name _THERMOMETER2_ in the Local Cloud, to make sure that the device it is running on is already registered. (Otherwise you must register the device first, as shown in [example 2](#step-2-register-and-revoke-device).)

The request looks like this:
~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/device-discovery/lookup' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "deviceNames": [
    "THERMOMETER2"
  ],
  "addresses": [
  ],
  "addressType": "",
  "metadataRequirementList": [
  ]
}'
~~~

Which leads to the following response from the ServiceRegistry:

~~~
{
  "entries": [
    {
      "name": "THERMOMETER2",
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
        }
      },
      "addresses": [
        {
          "type": "MAC",
          "address": "81:ef:1a:44:7a:f5"
        }
      ],
      "createdAt": "2024-11-04T01:53:02Z",
      "updatedAt": "2024-11-04T01:53:02Z"
    }
  ],
  "count": 1
}
~~~

Since we have an existing entity back, _THERMOMETER2_ is indeed a registered device.

### Step 3: Register System

Now we can perform the registration operation of our _TemperatureProvider2_ system.

To register, we need to provide the following information about the system:

- **metadata:** It's totally up to us what we put into it. For _TemperatureProvider2_, we want to make sure, that it contains a list about the temperature scales that the system can manage (Kelvin and Celsius), a location in the greenhouse from which the system's device measures the temperature (North side, 2. block), and the type of the thermometer (indoor).
- **version:** Since there was no prior version of this system, we can leave this field empty. The ServiceRegistry will initialize this value to a default, which is 1.0.0.
- **addresses:** This is typically an IP address or a hostname for the system. In case of the _TemperatureProvider2_, we want to provide both. For IP address, we are using 192.168.56.116, and the hostname is tp2.greenhouse.com.
- **device name:** This can be left blank, but we will set it to the name of our device: THERMOMETER2.

It is not necessary to specify the system name explicitly, because the ServiceRegistry extracts it from the authorization header.

Based on the above, the request looks like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/system-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "metadata": {
    "scales": [ "Kelvin", "Celsius" ],
    "location": { "side": "North", "block": 2 },
    "indoor": true
  },
  "version": "",
  "addresses": [
    "192.168.56.116",
    "tp2.greenhouse.com"
  ],
  "deviceName": "THERMOMETER2"
}'
~~~

We receive the following response:

~~~
{
  "name": "TemperatureProvider2",
  "metadata": {
    "scales": [
      "Kelvin",
      "Celsius"
    ],
    "location": {
      "side": "North",
      "block": 2
    },
    "indoor": true
  },
  "version": "1.0.0",
  "addresses": [
    {
      "type": "IPV4",
      "address": "192.168.56.116"
    },
    {
      "type": "HOSTNAME",
      "address": "tp2.greenhouse.com"
    }
  ],
  "device": {
    "name": "THERMOMETER2",
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
      }
    },
    "addresses": [
      {
        "type": "MAC",
        "address": "81:ef:1a:44:7a:f5"
      }
    ],
    "createdAt": "2024-11-04T01:53:02Z",
    "updatedAt": "2024-11-04T01:53:02Z"
  },
  "createdAt": "2024-11-08T10:21:10.950683800Z",
  "updatedAt": "2024-11-08T10:21:10.950683800Z"
}
~~~

### Step 4: Register Service Instances

The _TemperatureProvider2_ system that we have just registered provides three services in the Local Cloud:

- **kelvinInfo:** provides temperature information using the Kelvin scale,
- **celsiusInfo:** provides temperature information using the Celsius scale,
- **alertService:** sends an alert if the temperature is extreme (by default, these thresholds are 10 and 25 Celsius, but the consumer can overwrite them).

We have to register these services one by one.

**1. kelvinInfo:** 
We have to provide the following information:

- **service definition name:** In this example, we use kelvinInfo, which is already an existing service definition stored in the Local Cloud. You can use a non-existing service definition as well, because in that case, the registration process will create it.
- **version:** We will use the default version, so we can leave this field blank.
- **expires at:** This is a timestamp in the future, when the service is no longer funtioning. For kelvinInfo, we set this to 01. 01. 2030. 
- **metadata:** This can be customised depending on the service. For temperature information, we define the margin of error, which is 0.5 degree.
- **interfaces:** All the services use HTTP protocol, so we will go with the template named generic_http, that already exists in the Local Cloud. Note that in our case, the service discovery interface policy is set to _restricted_, which means that only already existing interface templates can be used. If you set this to _extendable_ or _open_, you can use non-existent interface templates, and they will be created as well. The interface provided by the kelvinInfo service is the following:
    - GET tp2.greenhouse.com:8080/kelvin/query
  
Based on these specifications, the request looks like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "serviceDefinitionName": "kelvinInfo",
  "version": "",
  "expiresAt": "2030-01-01T00:00:00Z",
  "metadata": {
    "marginOfError": 0.5
  },
  "interfaces": [
    {
      "templateName": "generic_http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": [ "192.168.56.116", "tp2.greenhouse.com" ],
        "accessPort": 8080,
        "basePath": "/kelvin",
        "operations": { "query-temperature": { "method": "GET", "path": "/query" } }
      }
    }
  ]
}'
~~~

After successful operation, we will receive the JSON object representation of the registered service:
~~~
{
  "instanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
  "provider": {
    "name": "TemperatureProvider2",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "location": {
        "side": "North",
        "block": 2
      },
      "indoor": true
    },
    "version": "1.0.0",
    "addresses": [
      {
        "type": "IPV4",
        "address": "192.168.56.116"
      },
      {
        "type": "HOSTNAME",
        "address": "tp2.greenhouse.com"
      }
    ],
    "device": {
      "name": "THERMOEMTER2",
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
        }
      },
      "addresses": [
        {
          "type": "MAC",
          "address": "81:ef:1a:44:7a:f5"
        }
      ],
      "createdAt": "2024-11-04T01:53:02Z",
      "updatedAt": "2024-11-04T01:53:02Z"
    },
    "createdAt": "2024-11-08T10:21:11Z",
    "updatedAt": "2024-11-08T10:21:11Z"
  },
  "serviceDefinition": {
    "name": "kelvinInfo",
    "createdAt": "2024-11-08T11:24:43Z",
    "updatedAt": "2024-11-08T11:24:43Z"
  },
  "version": "1.0.0",
  "expiresAt": "2030-01-01T00:00:00Z",
  "metadata": {
    "marginOfError": 0.5
  },
  "interfaces": [
    {
      "templateName": "generic_http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": [
          "192.168.56.116",
          "tp2.greenhouse.com"
        ],
        "accessPort": 8080,
        "operations": {
          "query-temperature": {
            "path": "/query",
            "method": "GET"
          }
        },
        "basePath": "/kelvin"
      }
    }
  ],
  "createdAt": "2024-11-19T12:00:07.959849300Z",
  "updatedAt": "2024-11-19T12:00:07.959849300Z"
}
~~~

**2. celsiusInfo:** 
The only difference with the kelvinInfo is the service definition name (celsiusInfo) and the interface (GET tp2.greenhouse.com:8080/celsius/query). All the other registation data will remain the same.
  
So the request will look like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "serviceDefinitionName": "celsiusInfo",
  "version": "",
  "expiresAt": "2030-01-01T00:00:00Z",
  "metadata": {
    "marginOfError": 0.5
  },
  "interfaces": [
    {
      "templateName": "generic_http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": [ "192.168.56.116", "tp2.greenhouse.com" ],
        "accessPort": 8080,
        "basePath": "/celsius",
        "operations": { "query-temperature": { "method": "GET", "path": "/query"} }
      }
    }
  ]
}'
~~~

The received response is the following:
~~~
{
  "instanceId": "TemperatureProvider2|celsiusInfo|1.0.0",
  "provider": {
    "name": "TemperatureProvider2",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "location": {
        "side": "North",
        "block": 2
      },
      "indoor": true
    },
    "version": "1.0.0",
    "addresses": [
      {
        "type": "IPV4",
        "address": "192.168.56.116"
      },
      {
        "type": "HOSTNAME",
        "address": "tp2.greenhouse.com"
      }
    ],
    "device": {
      "name": "THERMOMETER2",
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
        }
      },
      "addresses": [
        {
          "type": "MAC",
          "address": "81:ef:1a:44:7a:f5"
        }
      ],
      "createdAt": "2024-11-04T01:53:02Z",
      "updatedAt": "2024-11-04T01:53:02Z"
    },
    "createdAt": "2024-11-08T10:21:11Z",
    "updatedAt": "2024-11-08T10:21:11Z"
  },
  "serviceDefinition": {
    "name": "celsiusInfo",
    "createdAt": "2024-10-24T21:48:36Z",
    "updatedAt": "2024-10-24T21:48:36Z"
  },
  "version": "1.0.0",
  "expiresAt": "2030-01-01T00:00:00Z",
  "metadata": {
    "marginOfError": 0.5
  },
  "interfaces": [
    {
      "templateName": "generic_http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": [
          "192.168.56.116",
          "tp2.greenhouse.com"
        ],
        "accessPort": 8080,
        "operations": {
          "query-temperature": {
            "path": "/query",
            "method": "GET"
          }
        },
        "basePath": "/celsius"
      }
    }
  ],
  "createdAt": "2024-11-19T16:56:06.439181300Z",
  "updatedAt": "2024-11-19T16:56:06.439181300Z"
}
~~~

**3. alertService:** 

Our last service will be responsible for sending error messages. The registration data is the following:

- **service definition name:** In this case this is alertService.
- **version:** We will use the default version.
- **expires at:** The alertService expires a bit earlier than the previous ones, so we set this to 01. 01. 2027.
- **metadata:** For alert service, the maximum possible delay is given, which is 15 sec.
- **interfaces:** The interface's endpoints provided by the alert service are the following:
    - POST tp2.greenhouse.com:8000/alert/subscribe 
    - DELETE tp2.greenhouse.com:8000/alert/unsubscribe
    - POST tp2.greenhouse.com:8000/alert/threshold

We will register this service with the following request:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "serviceDefinitionName": "alertService",
  "version": "",
  "expiresAt": "2027-01-01T00:00:00Z",
  "metadata": {
    "maxDelay": { "value": 15, "unit": "sec" }
  },
  "interfaces": [
    {
      "templateName": "generic_http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": [ "192.168.56.116", "tp2.greenhouse.com" ],
        "accessPort": 8000,
        "basePath": "/alert",
        "operations": {
          "subscribe": { "method": "POST", "path": "/subscribe" },
          "unsubscribe": { "method": "DELETE", "path": "/unsubscribe" },
          "set-threshold": { "method": "POST", "path": "/threshold" }
        }
      }
    }
  ]
}'
~~~

The received answer:
~~~
{
  "instanceId": "TemperatureProvider2|alertService|1.0.0",
  "provider": {
    "name": "TemperatureProvider2",
    "metadata": {
      "scales": [
        "Kelvin",
        "Celsius"
      ],
      "location": {
        "side": "North",
        "block": 2
      },
      "indoor": true
    },
    "version": "1.0.0",
    "addresses": [
      {
        "type": "IPV4",
        "address": "192.168.56.116"
      },
      {
        "type": "HOSTNAME",
        "address": "tp2.greenhouse.com"
      }
    ],
    "device": {
      "name": "THERMOMETER2",
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
        }
      },
      "addresses": [
        {
          "type": "MAC",
          "address": "81:ef:1a:44:7a:f5"
        }
      ],
      "createdAt": "2024-11-04T01:53:02Z",
      "updatedAt": "2024-11-04T01:53:02Z"
    },
    "createdAt": "2024-11-08T10:21:11Z",
    "updatedAt": "2024-11-08T10:21:11Z"
  },
  "serviceDefinition": {
    "name": "alertService",
    "createdAt": "2024-11-08T15:23:10Z",
    "updatedAt": "2024-11-08T15:23:10Z"
  },
  "version": "1.0.0",
  "expiresAt": "2027-01-01T00:00:00Z",
  "metadata": {
    "maxDelay": {
      "value": 15,
      "unit": "sec"
    }
  },
  "interfaces": [
    {
      "templateName": "generic_http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": [
          "192.168.56.116",
          "tp2.greenhouse.com"
        ],
        "accessPort": 8000,
        "operations": {
          "subscribe": {
            "path": "/subscribe",
            "method": "POST"
          },
          "unsubscribe": {
            "path": "/unsubscribe",
            "method": "DELETE"
          },
          "set-threshold": {
            "path": "/threshold",
            "method": "POST"
          }
        },
        "basePath": "/alert"
      }
    }
  ],
  "createdAt": "2024-11-19T17:08:47.706960300Z",
  "updatedAt": "2024-11-19T17:08:47.706960300Z"
}
~~~

### Step 5: Lookup and Revoke Service 

Let's say  we have decided to no longer provide _TemperatureProvider2_'s  alertService. 

For deleting the service, we have to know the service instance ID, which was generated by the ServiceRegistry, when we registered our service into the Local Cloud. We can find out what the ID is, if we perform a lookup operation. Of course, if we know the ID, this step can be skipped.

Since we know that the provider name is _TemperatureProvider2_, the service definition name is alertService, and the version was 1.0.0, we will send a lookup request with these filters. We will set the _verbose_ flag to false, because we only need the service instance ID here, all the other additional data is unnecessary.

Note that our case, the service discovery policy is set to _OPEN_. If the discovery policy is _RESTRICTED_, we will only retrieve the services that have the metadata key _unrestrictedDiscovery_, and this is set to _true_.

The lookup request looks like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/lookup?verbose=false' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "instanceIds": [
  ],
  "providerNames": [
    "TemperatureProvider2"
  ],
  "serviceDefinitionNames": [
    "alertService"
  ],
  "versions": [
    "1.0.0"
  ],
  "alivesAt": "",
  "metadataRequirementsList": [
  ],
  "interfaceTemplateNames": [
  ],
  "interfacePropertyRequirementsList": [
  ],
  "policies": [
  ]
}'
~~~

The lookup response:

~~~
{
  "entries": [
    {
      "instanceId": "TemperatureProvider2|alertService|1.0.0",
      "provider": {
        "name": "TemperatureProvider2",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "location": {
            "side": "North",
            "block": 2
          },
          "indoor": true
        },
        "version": "1.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.56.116"
          },
          {
            "type": "HOSTNAME",
            "address": "tp2.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER2"
        },
        "createdAt": "2024-11-08T10:21:11Z",
        "updatedAt": "2024-11-08T10:21:11Z"
      },
      "serviceDefinition": {
        "name": "alertService",
        "createdAt": "2024-11-08T15:23:10Z",
        "updatedAt": "2024-11-08T15:23:10Z"
      },
      "version": "1.0.0",
      "expiresAt": "2025-01-01T00:00:00Z",
      "metadata": {
        "maxDelay": {
          "value": 15,
          "unit": "sec"
        }
      },
      "interfaces": [
        {
          "templateName": "generic_http",
          "protocol": "http",
          "policy": "NONE",
          "properties": {
            "accessAddresses": [
              "192.168.56.116",
              "tp2.greenhouse.com"
            ],
            "accessPort": 8000,
            "operations": {
              "subscribe": {
                "path": "/subscribe",
                "method": "POST"
              },
              "unsubscribe": {
                "path": "/unsubscribe",
                "method": "DELETE"
              },
              "set-threshold": {
                "path": "/threshold",
                "method": "POST"
              }
            },
            "basePath": "/alert"
          }
        }
      ],
      "createdAt": "2024-11-19T17:08:48Z",
      "updatedAt": "2024-11-19T17:08:48Z"
    }
  ],
  "count": 1
}
~~~

Since there was exactly one response, we learned that the service instance id is _TemperatureProvider2|alertService|1.0.0_. We can revoke the service by sending the following request:

~~~
curl -X 'DELETE' \
  'http://localhost:8443/serviceregistry/service-discovery/revoke/TemperatureProvider2%7CalertService%7C1.0.0' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2'
~~~

Note, the service instance id contains pipes (|) as separator characters, which has special meanings on some operation systems. Because of that, the pipe has to be encoded as %7C, that is why we used these in the URL.

This operation only works, if the service we are deleting is associated with our system. Otherwise, the following error message will be received:
~~~
{
  "errorMessage": "Revoking other systems' service is forbidden",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN",
  "origin": "DELETE /serviceregistry/service-discovery/revoke/TemperatureProvider2|alertService|1.0.0"
}
~~~

### Step 6:  Revoke System

Maybe later we want to revoke the _TemperatureProvider2_ because we will not use it anymore. We can remove the system from the Local Cloud by using the revoke system operation:
~~~
curl -X 'DELETE' \
  'http://localhost:8443/serviceregistry/system-discovery/revoke' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer SYSTEM//TemperatureProvider2'
~~~

The ServiceRegistry will delete our system.

## Example 2: Consumer

In this example, we'll see how:

- a **system** called _TemperatureConsumer1_, 
- running on the **device** named _WEATHER\_DISPLAYER1_,
- looks up for available _kelvinInfo_ **services**,

after registering itself and its device into the Local Cloud.

### Step 1: Authentication

Similar to the [providers's authentication](#step-1-authentication), we will use _self declared authentication_, and use the following authorization header:
~~~
-H 'Authorization: Bearer SYSTEM//TemperatureConsumer1'
~~~

### Step 2: Register and Revoke Device

Before registrating the system, we will register our device, because it doesn't exist in the Local Cloud yet. We provide the following information about the device:

- **name:** WEATHER\_DISPLAYER1
- **metadata:** We specify here that the displayer type is digital, and it is capable to display data about temperature and wind.
- **addresses:** The device has a MAC address, which is 57:2a:b8:ff:c8:f1.

The request will look like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/device-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureConsumer1' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "WEATHER_DISPLAYER1",
  "metadata": {
    "type": "digital",
    "displayedData": [ "wind", "temperature" ]
  },
  "addresses": [
    "57:2a:b8:ff:c8:f1"
  ]
}'
~~~

After successful registration, we receive the following response:

~~~
{
  "name": "WEATHER_DISPLAYER1",
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
      "address": "57:2a:b8:ff:c8:f1"
    }
  ],
  "createdAt": "2024-11-10T15:36:53.720922100Z",
  "updatedAt": "2024-11-10T15:36:53.720922100Z"
}
~~~

If for some reason we want to delete this device in the future, we can do this by sending the following request:
~~~
curl -X 'DELETE' \
  'http://localhost:8443/serviceregistry/device-discovery/revoke/WEATHER_DISPLAYER1' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer SYSTEM//TemperatureConsumer1'
~~~

Note that this operation will only be successful, if no system is connected to the device anymore. Otherwise you will get the following error message:
~~~
{
  "errorMessage": "At least one system is assigned to this device",
  "errorCode": 423,
  "exceptionType": "LOCKED",
  "origin": "DELETE /serviceregistry/device-discovery/revoke/WEATHER_DISPLAYER1"
}
~~~

In this example, we will not delete the device yet, because _TemperatureConsumer1_ runs on it.

### Step 3: Register System

Now, that we registered the device, we will register the system too with the following data:

- **metadata:** The system uses Kelvin, Celsius and Fahrenheit scales. It is placed indoor and is located in room 14. 
- **version:** This is the second version of this system. We can set it to 2, the ServiceRegistry will normalize it later according to semantic versioning.
- **addresses:** We specify an IP address (192.168.49.1) and the hostname (tc1.greenhouse.com).
- **device name:** This will be WEATHER_DISPLAYER1, which we registered earlier.

The registation request:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/system-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureConsumer1' \
  -H 'Content-Type: application/json' \
  -d '{
  "metadata": {
    "scales": [ "Kelvin", "Celsius", "Fahrenheit" ],
    "location": { "room": 14 },
    "indoor": true
  },
  "version": "2",
  "addresses": [
    "192.168.49.1",
    "tc1.greenhouse.com"
  ],
  "deviceName": "WEATHER_DISPLAYER1"
}'
~~~

Here is the response we received:

~~~
{
  "name": "TemperatureConsumer1",
  "metadata": {
    "scales": [
      "Kelvin",
      "Celsius",
      "Fahrenheit"
    ],
    "location": {
      "room": 14
    },
    "indoor": true
  },
  "version": "2.0.0",
  "addresses": [
    {
      "type": "IPV4",
      "address": "192.168.49.1"
    },
    {
      "type": "HOSTNAME",
      "address": "tc1.greenhouse.com"
    }
  ],
  "device": {
    "name": "WEATHER_DISPLAYER1",
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
        "address": "57:2a:b8:ff:c8:f1"
      }
    ],
    "createdAt": "2024-11-10T15:36:54Z",
    "updatedAt": "2024-11-10T15:36:54Z"
  },
  "createdAt": "2024-11-10T16:05:55.973663300Z",
  "updatedAt": "2024-11-10T16:05:55.973663300Z"
}
~~~

### Step 4: Lookup Services

Let's say that the _TemperatureConsumer1_ wants to consume the kelvinInfo service. We have to perform a lookup operation to find out the service instances registered into the Local Cloud with the corresponding service definition name. We are looking for services, that will not expire until 31. december 2025, and the margin of error is not higher than 1 degree.

We will use the following filters during lookup:

- **Service definition names:** We are looking for services named kelvinInfo.
- **Alives at:** This should be the UTC string representation of 31. 12. 2025.
- **Metadata requirements list**: As specified, the margin of error sould be less than or equals to 1. However, the service might not contain this metadata, if the measured temperature is always perfectly accurate. In this case, there has to be a _reliabe_ flag which is set to _true_. So we will have two metadata requirements, one is a limit for the margin of error metadata, and the other one is to check the _reliable_ flag. 

We will set the _verbose_ parameter to _true_, because we want all the possible details about the services, so we can make an optimal decision, which one to consume.

As mentioned in [example 1](#step-5-lookup-and-revoke-service), the service discovery policy here is set to OPEN. If the discovery policy is RESTRICTED, we may face some restrictions.

The request looks like this:
~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/lookup?verbose=true' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//TemperatureConsumer1' \
  -H 'Content-Type: application/json' \
  -d '{
  "instanceIds": [
  ],
  "providerNames": [
  ],
  "serviceDefinitionNames": [
    "kelvinInfo"
  ],
  "versions": [
  ],
  "alivesAt": "2025-12-31T00:00:00Z",
  "metadataRequirementsList": [
    {
      "marginOfError": { "op": "LESS_THAN_OR_EQUALS_TO", "value": 1}
    },
    {
      "reliable": true
    }
  ],
  "interfaceTemplateNames": [
  ],
  "interfacePropertyRequirementsList": [
  ],
  "policies": [
  ]
}'
~~~

The result contains two entities, TemperatureProvider2|kelvinInfo|1.0.0 and TemperatureProvider1|kelvinInfo|1.0.0:
~~~
{
  "entries": [
    {
      "instanceId": "TemperatureProvider2|kelvinInfo|1.0.0",
      "provider": {
        "name": "TemperatureProvider2",
        "metadata": {
          "scales": [
            "Kelvin",
            "Celsius"
          ],
          "location": {
            "side": "North",
            "block": 2
          },
          "indoor": true
        },
        "version": "1.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.56.116"
          },
          {
            "type": "HOSTNAME",
            "address": "tp2.greenhouse.com"
          }
        ],
        "device": {
          "name": "THERMOMETER2",
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
            }
          },
          "addresses": [
            {
              "type": "MAC",
              "address": "81:ef:1a:44:7a:f5"
            }
          ],
          "createdAt": "2024-11-04T01:53:02Z",
          "updatedAt": "2024-11-04T01:53:02Z"
        },
        "createdAt": "2024-11-08T10:21:11Z",
        "updatedAt": "2024-11-08T10:21:11Z"
      },
      "serviceDefinition": {
        "name": "kelvinInfo",
        "createdAt": "2024-11-08T11:24:43Z",
        "updatedAt": "2024-11-08T11:24:43Z"
      },
      "version": "1.0.0",
      "expiresAt": "2030-01-01T00:00:00Z",
      "metadata": {
        "marginOfError": 0.5
      },
      "interfaces": [
        {
          "templateName": "generic_http",
          "protocol": "http",
          "policy": "NONE",
          "properties": {
            "accessAddresses": [
              "192.168.56.116",
              "tp2.greenhouse.com"
            ],
            "accessPort": 8080,
            "operations": {
              "query-temperature": {
                "path": "/query",
                "method": "GET"
              }
            },
            "basePath": "/kelvin"
          }
        }
      ],
      "createdAt": "2024-11-10T14:24:13Z",
      "updatedAt": "2024-11-10T22:43:19Z"
    },
    {
      "instanceId": "TemperatureProvider1|kelvinInfo|1.0.0",
      "provider": {
        "name": "TemperatureProvider1",
        "metadata": {
          "scales": [
            "Kelvin"
          ],
          "location": {
            "side": "East",
            "block": 2
          },
          "indoor": false
        },
        "version": "1.0.0",
        "addresses": [
          {
            "type": "IPV4",
            "address": "192.168.56.110"
          },
          {
            "type": "HOSTNAME",
            "address": "tp1.greenhouse.com"
          }
        ],
        "createdAt": "2024-11-10T16:15:23Z",
        "updatedAt": "2024-11-10T16:15:23Z"
      },
      "serviceDefinition": {
        "name": "kelvinInfo",
        "createdAt": "2024-11-08T11:24:43Z",
        "updatedAt": "2024-11-08T11:24:43Z"
      },
      "version": "1.0.0",
      "expiresAt": "2030-05-31T00:00:00Z",
      "metadata": {
        "reliable": true
      },
      "interfaces": [
        {
          "templateName": "generic_http",
          "protocol": "http",
          "policy": "NONE",
          "properties": {
            "accessAddresses": [
              "192.168.56.110",
              "tp1.greenhouse.com"
            ],
            "accessPort": 8080,
            "operations": {
              "query-temperature": {
                "path": "/info",
                "method": "GET"
              }
            },
            "basePath": "/kelvin"
          }
        }
      ],
      "createdAt": "2024-11-10T21:08:21Z",
      "updatedAt": "2024-11-10T22:43:19Z"
    }
  ],
  "count": 2
}
~~~

Now we are free to choose, which instance of kelvinInfo service to consume.