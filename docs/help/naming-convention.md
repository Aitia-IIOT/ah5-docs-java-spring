# Naming Convention

Names of systems, services, operations, devices and interfaces are used in programing languages, URLs, DNS entries and file systems. Adopting a consistent naming convention across the community enhances interoperability among different programming environments and facilitates clearer interpretation by both humans and machines. Within the Arrowhead architecture, it is essential that key entities are clearly and distinctly identifiable through a well-defined and systematic naming strategy.

Learn more from the [Eclipse Arrowhead Naming Convention](../assets/fundamentals/naming-convention.pdf) fundamental document.

## Entities

Entity names could be **maximum of 63 characters long** and have to follow the below detailed naming style:

Entity | Naming Style | Example
--- | --- | ---
System | PascalCase | `ServiceRegistry`
Service | camelCase | `serviceDiscovery`
Service operation | kebab-case | `get-entries`
Interface | snake_case | `generic_http`
Device | UPPER_SNAKE_CASE | `MY_DEVICE` 

## Composite identifiers

Composite identifiers are made of multiple entity names and/or attributes delimited by `|`. **Each part can be maximum of 63 characters long**.

Identifier | Nameing Style | Example
--- | --- | --
Service instance | SystemName\|serviceName\|version | `ServiceRegistry|serviceDiscovery|1.0.0`
Cloud | CloudName\|OrganizationName | `TestCloud|AitiaInc`

## Special cases

### X.509 Certificates

The use of Common Name (CN) in X.509 Certificates - which is important in case of [certificate authentication policy](../api/authentication_policy.md#certificate) -  has limited capabilities. To overcome this issue, the common name parts in the certificates can follow the kebab styling and be delimited by `.` character. The common name will be transformed to the appropriate styling on code level, and from then on this transformed version will be processed.

System profile | example
--- | ---
Required CN | `my-provider.my-cloud.my-company.arrowhead.eu`
Transformed CN | `MyProvider.MyCloud.MyCompany.arrowhead.eu`
Retrieved System Name | `MyProvider`
Retrieved Cloud Identifier | `MyCloud|MyCompany`

Device profile | example
--- | ---
Required CN | `my-device.my-cloud.my-company.arrowhead.eu`
Transformed CN | `MY_DEVICE.MyCloud.MyCompany.arrowhead.eu`
Retrieved Device Name | `MY_DEVICE`
Retrieved Cloud Identifier | `MyCloud|MyCompany`