# General Configuartion Properties

The following cofiguration properties are applied by (almost) every core and support system:

:fontawesome-solid-wrench: **authentication.policy**

Specifies the applied [authentication policy](../api/authentication_policy.md). Can be `declared`, `certificate` and `outsourced`. 

:fontawesome-solid-wrench: **server.address**

IP address of the server using HTTP(S) protocol (0.0.0.0 denotes all available IP addresses).

:fontawesome-solid-wrench: **server.port**

Port number of the server for HTTP(S) protocol.

:fontawesome-solid-wrench: **domain.name**

The address the system will use to register itself into the local cloud's Service Registry.

:fontawesome-solid-wrench: **service.registry.address**

HTTP(S) Access address of the local cloud's Service Registry system. In case of the Service Registry itself, this property is not specified.

:fontawesome-solid-wrench: **service.registry.port**

HTTP(S) Access port of the local cloud's Service Registry system. In case of the Service Registry itself, this property is not specified.