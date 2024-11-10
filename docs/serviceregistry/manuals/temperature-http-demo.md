# Temperature http demo

## Recap

In this demo, we will demonstrate through a concrete example how a system should use the Service Registry endpoints.

Let's say, we have a greenhouse. Inside the greenhouse, there are plantations  divided into blocks, and close to the blocks, _thermometers_ are placed. We can monitor the temperature of the blocks, using the _systems_ that run on the _thermometers_. If the temperature is too extreme, the _systems_ may send a warning to our office.

There are two types of _thermometers_: indoor and outdoor _thermometers_, so we can even monitor the temperature outside the greenhouse.

In our office, there are different _weather displayers_ that can show various data about the weather, such as wind or temperature. The temperature data measured by the thermometers in the greenhouse are sent to these _weather displayers_. We also have _alarms_, this way we are immediately informed of extreme temperatures.

_Thermometers_ can provide information on temperature in Celsius, Fahrenheit or Kelvin scales, but not all _thermometers_ are capable of sending data at all scales.

The concrete entities used in our example can be mapped to Service Registry entities as follows:

- **Devices:** _thermometers_, _weather displayers_ and _alarms_
- **Systems:** _temperature providers_ running on the thermometers, _temperature consumers_ running on the weather displayers, and _alert consumers_ running on the alarms.
- **Services:** _Fahrenheit info_, _Kelvin info_, _Celsius info_, and _alert service_, provided by the temperature providers and consumed by the temperature consumers and the alert consumers

We will demonstrate the usage of the endpoints via two examples: [example 1](#example-1:-provider) is about a provider system, and [example 2](#example-2:-consumer) is about a consumer system.

## Example 1: Provider

In the following, we'll see how:
- a **system** called _temperature-provider-2_, 
- running on the **device** named _thermometer2_,
- publishes its **services:** _Kelvin-info_, _Celsius-info_ and _alert-service_

after registering itself into the Local Cloud.

### Step 1: Authentication

First of all, the system should perform some kind of authentication. In Arrowhead 5, there are three ways for a system to authenticate itself: involving an Authentication Core System, using X.509 Certicifates, or telling who they are by themselves. In this example, the last one will be used, which is called _self-declared authentication_.

To perform this type of authentication, the system must provide an authentication header for each request. This should consist of the _SYSTEM//_ prefix, followed by the name of the system. 

In our example, the authorization header will look like this:
~~~
-H 'Authorization: Bearer SYSTEM//temperature-provider-2'
~~~

### Step 2: Lookup devices

During registration, the system can specify which device it is running on. In this case, our device is called _thermometer2_. Specifying the device is optional, but let's say, that in our example it is relevant to the operation of _temperature-provider-2_. Therefore, the system first looks up if there is a device with name _thermometer2_ in the Local Cloud, to make sure that the device it is running on is already registered. (Otherwise, you must register the device first.)

The request looks like this:
~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/device-discovery/lookup' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//temperature-provider-2' \
  -H 'Content-Type: application/json' \
  -d '{
  "deviceNames": [
    "thermometer2"
  ],
  "addresses": [
  ],
  "addressType": "",
  "metadataRequirementList": [
  ]
}'
~~~

Which leads to the following response from the Service Registry:

~~~
{
  "entries": [
    {
      "name": "thermometer2",
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

Since we have an existing entity back, _thermometer2_ is indeed a registered device.

### Step 3: Register system

Now we can perform the registration operation of our _temperature-provider2_ system.

To register, we need to provide the following information about the system:
- **metadata:** It's totally up to us what we put into it. For _temperature-provider2_, we want to make sure, that it contains a list about the temperature scales that the system can manage (Kelvin and Celsius), a location in the greenhouse from which the system's device measures the temperature (North side, 2. block), and the type of the thermometer (indoor).
- **version:** Since there was no prior version of this system, we can leave this field empty. The Service Registry will initialize this value to a default, which is 1.0.0.
- **addresses:** This is typically an IP address or a hostname for the system. In case of the _temperature-provider2_, we want to provide both. For IP address, we are using 192.168.56.116, and the hostname is tp2.greenhouse.com.
- **device name:** This can be left blank, but we will set it to the name of our device: thermometer2.

It is not necessary to specify the system name explicitly, because the Service Registry extracts it from the authorization header.

Based on the above, the request looks like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/system-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//temperature-provider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "metadata": {
    "scales": ["Kelvin", "Celsius"],
    "location": {"side": "North", "block": 2},
    "indoor": true
  },
  "version": "",
  "addresses": [
    "192.168.56.116",
    "tp2.greenhouse.com"
  ],
  "deviceName": "thermometer2"
}'
~~~

We receive the following response:

~~~
{
  "name": "temperature-provider2",
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
    "name": "thermometer2",
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

### Step 4: Register service instances

The _temperature-provider2_ system that we have just registered provides three services in the Local Cloud:
- **Kelvin info:** provides temperature information using the Kelvin scale,
- **Celsius info:** provides temperature information using the Celsius scale,
- **alert service:** sends an alert if the temperature is extreme (by default, these thresholds are 10 and 25 Celsius, but the consumer can overwrite them).

We have to registrer these services one by one.

**1. Kelvin info:** 
We have to provide the following information:
- **service definition name:** We have to use the name of the existing service definitions stored in the Local Cloud. In this example, this is kelvin-info.
- **version:** We will use the default version, so we can leave this field blank.
- **expires at:** This is a timestamp in the future, when the service is no longer funtioning. For Kelvin info, we set this to 01. 01. 2030. 
- **metadata:** This can be customised depending on the service. For temperature information, we define the margin of error, which is 0,5 degree.
- **interfaces:** All the services use http protocol, so we will go with the template named generic-http, that already exists in the Local Cloud. Note that in out case, the service discovery interface policy is set to _restricted_, which means that only already existing interface templates can be used. If you set this to _extendable_ or _open_, you can use non-existent interface templates, and they will be created as well. The interface provided by the Kelvin info service is the following:
  - GET tp2.greenhouse.com:8080/kelvin/query
  
Based on these specifications, the request looks like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//temperature-provider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "serviceDefinitionName": "kelvin-info",
  "version": "",
  "expiresAt": "2030-01-01T00:00:00Z",
  "metadata": {
    "margin-of-error": 0.5
  },
  "interfaces": [
    {
      "templateName": "generic-http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": ["192.168.56.116", "tp2.greenhouse.com"],
        "accessPort": 8080,
        "basePath": "/kelvin",
        "operations": {"query-temperature": { "method": "get", "path": "/query"} }
      }
    }
  ]
}'
~~~

**2. Celsius info:** 
The only difference with the Kelvin info is the service definition name (celsius-info) and the interface (GET tp2.greenhouse.com:8080/celsius/query). All the other registation data will remain the same.
  
The request will look like this:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//temperature-provider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "serviceDefinitionName": "celsius-info",
  "version": "",
  "expiresAt": "2030-01-01T00:00:00Z",
  "metadata": {
    "margin-of-error": 0.5
  },
  "interfaces": [
    {
      "templateName": "generic-http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": ["192.168.56.116", "tp2.greenhouse.com"],
        "accessPort": 8080,
        "basePath": "/celsius",
        "operations": {"query-temperature": { "method": "get", "path": "/query"} }
      }
    }
  ]
}'
~~~

**2. Alert service:** 
Our last service will be responsible for sending error messages. The registration data is the following:
- **service definition name:** In this case this is alert-service.
- **version:** We will use the default version.
- **expires at:** The alert service expires a bit earlier than the previous ones, so we set this to 01. 01. 2025.
- **metadata:** For alert serivce, the maximum possible delay is given, which is 15 sec.
- **interfaces:** The interfaces provided by Alert service are the following:
  - GET tp2.greenhouse.com:8000/alert/subscribe 
  - GET tp2.greenhouse.com:8000/alert/unsubscribe
  - POST tp2.greenhouse.com:8000/alert/threshold

We will register this service with the following request:

~~~
curl -X 'POST' \
  'http://localhost:8443/serviceregistry/service-discovery/register' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer SYSTEM//temperature-provider2' \
  -H 'Content-Type: application/json' \
  -d '{
  "serviceDefinitionName": "alert-service",
  "version": "",
  "expiresAt": "2025-01-01T00:00:00Z",
  "metadata": {
    "max-delay": {"value": 15, "unit": "sec"}
  },
  "interfaces": [
    {
      "templateName": "generic-http",
      "protocol": "http",
      "policy": "NONE",
      "properties": {
        "accessAddresses": ["192.168.56.116", "tp2.greenhouse.com"],
        "accessPort": 8080,
        "basePath": "/alert",
        "operations": {
          "subscribe": { "method": "get", "path": "/subscribe"},
          "unsubscribe": { "method": "get", "path": "/unsubscribe"},
          "set-threshold": { "method": "post", "path": "/threshold"}
        }
      }
    }
  ]
}'
~~~

### Step 5: Lookup and revoke service 

Let's say  we have decided to no longer provide _temperature-provider2_'s  alert service. 

For deleting the service, we have to know the service instance ID, which was generated by the Service Registry, when we registered our service into the Local Cloud. We can find out what the ID is, if we perform a lookup operation. Of course, if we know the ID, this step can be skipped.

Since we know that the provider name is _temperature-provider2_, the servide definition name is alert-service, and the version was 1.0.0, we will send a lookup request with these filters.

Note that our case, the service discovery policy is set to _OPEN_. If the discovery policy is _RESTRICTED_, we will only retrieve the services that have the  metadata key _unrestricted-discovery_, and this is set to _true_.

### Step 6:  Revoke system

## Example 2: Consumer

## Step 1:
