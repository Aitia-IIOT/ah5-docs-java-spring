# Temperature http demo

## Recap

In this demo, we will demonstrate through a concrete example how a system should use the Service Registry endpoints.

Let's say, we have a greenhouse. Inside the greenhouse, there are plantations  divided into blocks, and close to certain blocks, _thermometers_ are placed. We can monitor the temperature, using the _systems_ that run on them.

There are two types of _thermometers_: indoor and outdoor _thermometers_, so we can even monitor the temperature outside the greenhouse.

In our office, there are different _weather displayers_ that can show various data about the weather, such as wind or temperature. The temperature data measured by the thermometers in the greenhouse are sent to these _weather displayers_.

_Thermometers_ can provide information on temperature in Celsius, Fahrenheit or Kelvin scales, but not all _thermometers_ are capable of sending data at all scales.

The concrete entities used in our example can be mapped to Service Registry entities as follows:

- **Devices:** _thermometers_ and _weather displayers_
- **Systems:** _temperature providers_ running on the _thermometers_, and _temperature consumers_ running on the _weather displayers_
- **Services:** _Fahrenheit info_, _Kelvin info_ and _Celsius info_, provided by the _temperature providers_ and consumed by the _temperature consumers_

We will demonstrate the usage of the endpoints via two examples: [example 1](#example-1:-provider) is about a provider system, and [example 2](#example-2:-consumer) is about a consumer system.

## Example 1: Provider

In the following, we'll see how:
- a **system** called _temperature-provider-2_, 
- running on the **device** named _thermometer2_,
- publishes its **services:** _Kelvin-info_ and _Celsius-info_,

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

### Step 5: Register service instance

Now that we have our system registered, we can finally register the services it provides. Since we want to serve temperature information in Kelvin and Celsius, we will use the service definitions named kelvin-info and celsius-info.

### Step 6: Revoke service instance

### Step 7:  Revoke the system

## Example 2: Consumer

## Step 1:
