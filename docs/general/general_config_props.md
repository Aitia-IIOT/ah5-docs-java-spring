# General Configuartion Properties

The following cofiguration properties are applied by (almost) every Core and Support system:

:fontawesome-solid-wrench: **authentication.policy**

Specifies the applied [authentication policy](../api/authentication_policy.md). Can be `declared`, `certificate` or `outsourced`. 

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

:fontawesome-solid-wrench: **log.all.request.and.response**

Set to `true` in order to show all HTTP requests/responses in debug log.

:fontawesome-solid-wrench: **server.ssl.enabled**

Set to `true` in order to enable HTTPS mode.

:fontawesome-solid-wrench: **server.ssl.key-store-type**
 
Type of the key store. It should be PKCS12

:fontawesome-solid-wrench: **server.ssl.key-store**

Path to the key store.

:fontawesome-solid-wrench: **server.ssl.key-store-password**
 
Password to the key store.

:fontawesome-solid-wrench: **server.ssl.key-alias**

Alias name of the X.509 certificate.

:fontawesome-solid-wrench: **server.ssl.key-password**

Password to the certificate.

:fontawesome-solid-wrench: **server.ssl.client-auth**

Whether the clients of the system must send their certificate during service consumption or not. If **authentication.policy** is `certificate`, this property should be `need` which means that SSL client authentication is necessary. Otherwise, it should be `none`.

:fontawesome-solid-wrench: **server.ssl.trust-store-type**

Type of the trust store. It should be PKCS12.

:fontawesome-solid-wrench: **server.ssl.trust-store**
 
Path to the trust store.

:fontawesome-solid-wrench: **server.ssl.trust-store-password**

Password to the trust store.

:fontawesome-solid-wrench: **disable.hostname.verifier**

If `true`, HTTP client does not check whether the hostname is match one of the server's SAN (Subject Alternative Name) in its certificate. This should not be used in a production environment.

:fontawesome-solid-wrench: **mqtt.api.enabled**

If `true`, the services of the system can also be accessed via an MQTT broker.

:fontawesome-solid-wrench: **mqtt.broker.address**

Access address of the MQTT broker.

:fontawesome-solid-wrench: **mqtt.broker.port**

Access port of the MQTT broker.

:fontawesome-solid-wrench: **mqtt.client.password**

The system's password to gain access to the specified MQTT broker (the unique system name will be used as username during the login).
