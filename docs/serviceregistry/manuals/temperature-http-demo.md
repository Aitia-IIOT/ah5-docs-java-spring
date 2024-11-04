# Temperature http demo

## Recap

In this demo, we will demonstrate through a concrete example how a system should use the Service Registry endpoints.

Let's say we have to maintain a greenhouse with _thermometers_ placed at certain places. On the _thermometers_, systems are running, that monitor the temperature at different points in the greenhouse.

In our office, there are different _displayers_ that can show various data about the weather, such as wind or temperature. The data measured by the thermometers in the greenhouse are sent to these _weather displayers_.

_Thermometers_ can provide information on temperature in Celsius, Fahrenheit or Kelvin scales, but not all _thermometers_ are capable of sending data at all scales.

The concrete entities used in our example can be mapped to Service Registry entities as follows:

- **Devices:** _thermometers_ and _weather displayers_
- **Systems:** _temperature providers_ running on the _thermometers_, and _temperature consumers_ running on the _weather displayers_
- **Services:** _Fahrenheit info_, _Kelvin info_ and _Celsius info_, provided by the _temperature providers_ and consumed by the _temperature consumers_

In the following, we'll see how:
- a **system** called _temperature-provider-2_, 
- running on the **device** named _thermometer2_,
- publishes its **services:** _Kelvin-info_ and _Celsius-info_,

after registering itself into the Local Cloud.

## Step 1: Authentication

First of all, the system should perform some kind of authentication. In Arrowhead 5, there are three ways for a system to authenticate itself: involving an Authentication Core System, using X.509 Certicifates, or telling who they are by themselves. In this example, the last one will be used, which is called _self-declared authentication_.

To perform this type of authentication, the system must provide an authentication header for each request. This should consist of the _SYSTEM//_ prefix, followed by the name of the system. 

In our example, the authorization header will look like this:
~~~
-H 'Authorization: Bearer SYSTEM//temperature-provider-2'
~~~

## Step 2: Lookup devices

During registration, the system can specify which device it is running on. In this case, the device is called _thermometer2_. Specifying the device is optional, but in our example it is relevant to the operation of _temperature-provider-2_. Therefore, the system first looks up if there is a device with name _thermometer2_ in the Local Cloud, to make sure that the device it is running on is already registered.

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

## Step 3: Register system

## Step 4: Lookup service definitions

## Step 5: Register service instance

## Step 6: Revoke service instance

## Step 7:  Revoke the system
