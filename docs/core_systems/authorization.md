# ConsumerAuthorization

This Core system manages and authorizes connections between various systems using authorization rules within an Eclipse Arrowhead Local Cloud (LC).
It also provides various token generation functionalities that add an extra layer of security.

Learn more: <br />
:material-file-document: [Abstract System Description (SysD)](../assets/sysd/5_0_0/authorization_sysd.pdf)

## Services

### authorization

The purpose of this service is to validate service consumption permissions and to lookup, grant and revoke those permissions. The service is offered for both application and Core/Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-generic-http.md)<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-generic-mqtt.md)<br />
:material-tag: since: v5.0.0 

**grant**

This service operation enables a provider to grant access to various consumers to its service. It can also be used by a publisher to grant access to subscribers to its event with a specific type.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#grant) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#grant)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#grant) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#grant)

**revoke**

This service operation enables a provider/publisher to remove existing authorization policies that were created by itself.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#revoke) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#revoke)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#revoke) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#revoke)

**lookup**

This service operation lists the requester-created authorization rules that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#lookup) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#lookup)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#lookup) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#lookup)

**verify**

This service operation checks whether a consumer has access to a provider's specified service/service operation/event type.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-generic-http.md#verify) | [generic_https](../api/consumerauthorization/authorization-generic-http.md#verify)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-generic-mqtt.md#verify) | [generic_mqtts](../api/consumerauthorization/authorization-generic-mqtt.md#verify)

### authorizationToken

The purpose of this service is to generate and validate authorization tokens. The service is offered for both application and Core/Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization-token_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-token-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-token-generic-http.md)<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-token-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-token-generic-mqtt.md)<br />
:material-tag: since: v5.0.0 

**generate**

Its purpose is to verify the requester’s permissions and produce a token of defined type for the targeted service consumption.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-generic-http.md#generate) | [generic_https](../api/consumerauthorization/authorization-token-generic-http.md#generate)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-generic-mqtt.md#generate) | [generic_mqtts](../api/consumerauthorization/authorization-token-generic-mqtt.md#generate)

**verify**

Its purpose is to check whether a given token is valid or not, is associated with the requester or not and to provide the belonged token details.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-generic-http.md#verify) | [generic_https](../api/consumerauthorization/authorization-token-generic-http.md#verify)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-generic-mqtt.md#verify) | [generic_mqtts](../api/consumerauthorization/authorization-token-generic-mqtt.md#verify)

**get-public-key**

Its purpose is to provide the public key of the implementing system if any (necessary for the verification of some token types).

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-generic-http.md#get-public-key) | [generic_https](../api/consumerauthorization/authorization-token-generic-http.md#get-public-key)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-generic-mqtt.md#get-public-key) | [generic_mqtts](../api/consumerauthorization/authorization-token-generic-mqtt.md#get-public-key)

**register-encryption-key**

Its purpose is to store an encryption key that can be used to encrypt the raw tokens generated for any service of the requester system.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-generic-http.md#register-encryption-key) | [generic_https](../api/consumerauthorization/authorization-token-generic-http.md#register-encryption-key)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-generic-mqtt.md#register-encryption-key) | [generic_mqtts](../api/consumerauthorization/authorization-token-generic-mqtt.md#register-encryption-key)

**unregister-encryption-key**

Its purpose is to remove the encryption key belonged to the requester system.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-generic-http.md#unregister-encryption-key) | [generic_https](../api/consumerauthorization/authorization-token-generic-http.md#unregister-encryption-key)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-generic-mqtt.md#unregister-encryption-key) | [generic_mqtts](../api/consumerauthorization/authorization-token-generic-mqtt.md#unregister-encryption-key)

### generalManagement

Its purpose is to get some information about the hosting system's behavior, such as log entries and configuration settings. The service is offered for administrative Support systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/general-management_sd.pdf) <br />
:material-api: [generic_http (IDD)](../api/general/general-management-generic-http.md) | [generic_https (IDD)](../api/general/general-management-generic-http.md) <br />
:material-api: [generic_mqtt (IDD)](../api/general/general-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/general/general-management-generic-mqtt.md) <br />
:material-tag: since: v5.0.0 

**get-log**

This service operation lists the log entries of the system that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/general/general-management-generic-http.md#get-log) | [generic_https](../api/general/general-management-generic-http.md#get-log)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/general/general-management-generic-mqtt.md#get-log) | [generic_mqtts](../api/general/general-management-generic-mqtt.md#get-log)

**get-config**

This service operation lists the current values of the specified configuration settings.

:material-arrow-right-thin: Example: [generic_http](../api/general/general-management-generic-http.md#get-config) | [generic_https](../api/general/general-management-generic-http.md#get-config)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/general/general-management-generic-mqtt.md#get-config) | [generic_mqtts](../api/general/general-management-generic-mqtt.md#get-config)

### authorizationManagement

The purpose of this service is to manage (grant, revoke, query, check) authorization rules and validate service consumption permissions in bulk. The service is offered for Core and administrative Support systems.  

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization-management_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-management-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-management-generic-http.md)<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-management-generic-mqtt.md)<br />
:material-tag: since: v5.0.0 

**grant-policies**

This service operation enables a system with proper rights to grant access to various consumers for various provider's services in bulk. It can also be used to grant access to subscribers for various publisher's events with a specific type in bulk.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-management-generic-http.md#grant-policies) | [generic_https](../api/consumerauthorization/authorization-management-generic-http.md#grant-policies)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-management-generic-mqtt.md#grant-policies) | [generic_mqtts](../api/consumerauthorization/authorization-management-generic-mqtt.md#grant-policies)

**revoke-policies**

This service operation enables a system with proper rights to remove existing authorization policies in bulk without considering policy ownerships.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-management-generic-http.md#revoke-policies) | [generic_https](../api/consumerauthorization/authorization-management-generic-http.md#revoke-policies)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-management-generic-mqtt.md#revoke-policies) | [generic_mqtts](../api/consumerauthorization/authorization-management-generic-mqtt.md#revoke-policies)

**query-policies**

This service operation lists the authorization rules that match the filtering requirements. This operation can be used to query both provider-owned and management level authorization policies.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-management-generic-http.md#query-policies) | [generic_https](../api/consumerauthorization/authorization-management-generic-http.md#query-policies)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-management-generic-mqtt.md#query-policies) | [generic_mqtts](../api/consumerauthorization/authorization-management-generic-mqtt.md#query-policies)

**check-policies**

This service operation checks whether consumers have access to providers' specified service/service operation/event type.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-management-generic-http.md#check-policies) | [generic_https](../api/consumerauthorization/authorization-management-generic-http.md#check-policies)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-management-generic-mqtt.md#check-policies) | [generic_mqtts](../api/consumerauthorization/authorization-management-generic-mqtt.md#check-policies)

### authorizationTokenManagement

The purpose of this service is to manage (generate, revoke, query) authorization tokens in bulk. The service is offered for Core and administrative Support Systems.

Learn more: <br />
:material-file-document: [Abstract Service Description (SD)](../assets/sd/5_0_0/authorization-token-management_sd.pdf)<br />
:material-api: [generic_http (IDD)](../api/consumerauthorization/authorization-token-management-generic-http.md) | [generic_https (IDD)](../api/consumerauthorization/authorization-token-management-generic-http.md)<br />
:material-api: [generic_mqtt (IDD)](../api/consumerauthorization/authorization-token-management-generic-mqtt.md) | [generic_mqtts (IDD)](../api/consumerauthorization/authorization-token-management-generic-mqtt.md)<br />
:material-tag: since: v5.0.0 

**generate-tokens**

This service operation verifies the given consumer systems’ permissions to the targeted service/service-operation/event type instance and produces expiring access tokens for them.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-management-generic-http.md#generate-tokens) | [generic_https](../api/consumerauthorization/authorization-token-management-generic-https.md#generate-tokens)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-management-generic-mqtt.md#generate-tokens) | [generic_mqtts](../api/consumerauthorization/authorization-token-management-generic-mqtts.md#generate-tokens)

**query-tokens**

This service operation lists the access tokens that match the filtering requirements.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-management-generic-http.md#query-tokens) | [generic_https](../api/consumerauthorization/authorization-token-management-generic-https.md#query-tokens)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-management-generic-mqtt.md#query-tokens) | [generic_mqtts](../api/consumerauthorization/authorization-token-management-generic-mqtts.md#query-tokens)

**revoke-tokens**

This service operation deletes the access token records associated with the given token references.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-management-generic-http.md#revoke-tokens) | [generic_https](../api/consumerauthorization/authorization-token-management-generic-https.md#revoke-tokens)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-management-generic-mqtt.md#revoke-tokens) | [generic_mqtts](../api/consumerauthorization/authorization-token-management-generic-mqtts.md#revoke-tokens)

**add-encryption-keys**

This service operation saves and stores encryption key and algorithm identifier pairs for the given provider systems.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-management-generic-http.md#add-encryption-keys) | [generic_https](../api/consumerauthorization/authorization-token-management-generic-https.md#add-encryption-keys)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-management-generic-mqtt.md#add-encryption-keys) | [generic_mqtts](../api/consumerauthorization/authorization-token-management-generic-mqtts.md#add-encryption-keys)

**remove-encryption-keys**

This service operation deletes the stored encryption key and algorithm identifier pairs associated with the given provider systems.

:material-arrow-right-thin: Example: [generic_http](../api/consumerauthorization/authorization-token-management-generic-http.md#remove-encryption-keys) | [generic_https](../api/consumerauthorization/authorization-token-management-generic-https.md#remove-encryption-keys)<br />
:material-arrow-right-thin: Example: [generic_mqtt](../api/consumerauthorization/authorization-token-management-generic-mqtt.md#remove-encryption-keys) | [generic_mqtts](../api/consumerauthorization/authorization-token-management-generic-mqtts.md#remove-encryption-keys)

-----

## Configuration

The system configuration properties can be found in the `application.properties` file located at `/src/main/resources` folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but also going to be copied next to the JAR file. Any modification in the configuration file located next to the executable JAR file will override the built in configuration property value.

### General parameters

See the [general configuration properties](../general/general_config_props.md).

### Database parameters

:fontawesome-solid-wrench: **spring.datasource.url**

Full connection URL to the database.

:fontawesome-solid-wrench: **spring.datasource.username**

Username to the database.

:fontawesome-solid-wrench: **spring.datasource.password**

Password to the database.

:fontawesome-solid-wrench: **spring.datasource.driver-class-name**

The driver provides the connection to the database and implements the protocol for transferring the query
and result between client and database.

:fontawesome-solid-wrench: **spring.jpa.show-sql**

Set to `true` in order to log out the SQL queries.

:fontawesome-solid-wrench: **spring.jpa.properties.hibernate.format sql**

Set to `true` to log out SQL queries in pretty format. (Effective only when 'spring.jpa.show-sql' is 'true')

:fontawesome-solid-wrench: **spring.jpa.hibernate.ddl-auto**

Auto initialization of database tables. Value must be always 'none'.

### Custom parameters

:fontawesome-solid-wrench: **authenticator.credentials**

The credentials that this system will use for performing the login operation when the authentication policy is `outsourced`.

```
authenticator.credentials={\
	'<credential-name>': '<credential-value>' \
}
```

:fontawesome-solid-wrench: **authenticator.secret.keys**

Secret key for the authenticator servers when authentication policy is `outsourced`. The authenticator servers are able to use authorization check services by providing an HMAC of their system name computed with the associated secret key.

```
authenticator.secret.keys={\
	'<system-name>': '<secret-key>' \
}
```

:fontawesome-solid-wrench: **enable.management.filter**

Set to `true` to enable automatic authorization for management services.

:fontawesome-solid-wrench: **management.policy**

Defines the access policy for management services. Can be `sysop-only` (only systems with system operator permission can use them), `whitelist` (system operators and those dedicated systems that appear on the **management.whitelist** can use them) or `authorization` (system operators, whitelist members and those systems that have permission according to database-stored policies can use them).

:fontawesome-solid-wrench: **management.whitelist**

A list of system names (separated by comma) that can use management services if the **management.policy** is set to `whitelist` or `authorization`.

:fontawesome-solid-wrench: **enable.blacklist.filter**

Enable/disable automatic service requester system name verification against to cloud level blacklist. Can be `true` or `false`.

:fontawesome-solid-wrench: **force.blacklist.filter**

Whether or not the service requests should be refused when the blacklist server is not responding. Can be `true` or `false`.

:fontawesome-solid-wrench: **blacklist.check.exclude.list**

Comma-separated list that contains systems whose requests are served without checking the cloud level blacklist, even if blacklist is enabled.

:fontawesome-solid-wrench: **max.page.size**

Specifies the maximum number of records a page can contain in case of pageable service responses.

:fontawesome-solid-wrench: **token.max.age**

Determines after how long to delete the tokens from the database (in minutes).

:fontawesome-solid-wrench: **token.time.limit**

Specifies the default duration of time-limited tokens (simple time-limited and self-contained tokens) in seconds.

:fontawesome-solid-wrench: **simple.token.usage.limit**

Maximum token usage default in case of usage-limited simple tokens.

:fontawesome-solid-wrench: **unbounded.token.generation.whitelist**

Comma-separated list that contains the name of systems that can generate tokens for other systems by bypassing their authorization checks.

:fontawesome-solid-wrench: **simple.token.byte.size**

Simple token (time-limited, usage-limited) size in bytes. Cannot be less than 16!

:fontawesome-solid-wrench: **secret.cryptographer.key**

Sensitive data (consumer tokens, provider encryption keys) will be encrypted with this key before writing out into the database. Must be exactly 16 byte long!

:fontawesome-solid-wrench: **cleaner.job.interval**

Specifies how often (in miliseconds) to remove the expired and/or old tokens.

### Logging configuration

The logging configuration properties can be found in the `log4j2.xml` file located at `src/main/resources`
folder.

**_Note:_** During the build process this file is going to be built into the executable JAR, but it is also possible to
override it by an external file. For that use the following command when starting the system:
```
java -jar arrowhead-consumer-authentication-5.x.x.jar
     -Dlog4j.configurationFile=path-to-external-file
```

:fontawesome-solid-wrench: **JDBC_LEVEL**

Set this to change the level of log messages in the database. Levels: `ALL`, `TRACE`, `DEBUG`, `INFO`, `WARN`,
`ERROR`, `FATAL`, `OFF`.

:fontawesome-solid-wrench: **CONSOLE_FILE_LEVEL**

Set this to change the level of log messages in console and the log file. Levels: `ALL`, `TRACE`, `DEBUG`, `INFO`, `WARN`,
`ERROR`, `FATAL`, `OFF`.

:fontawesome-solid-wrench: **LOG_DIR**

Set this to change the directory of log files.

## Changelog

#### :material-tag: v5.0.0 

Related in CL-5.0.0

- [general](../general/changelogs/cl500.md#general)
- [arrowhead-common-utils](../general/changelogs/cl500.md#arrowhead-common-utils)
- [arrowhead-data-transfer-objects](../general/changelogs/cl500.md#arrowhead-data-transfer-objects)
- [arrowhead-consumer-authorization](../general/changelogs/cl500.md#arrowhead-consumer-authorization)