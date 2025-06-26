# AuthorizationVerifyRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
provider | [SystemName](../primitives.md#systemname) | no (yes) | The name of the system that provides the target. Mandatory if the _consumer_ is the requester.
consumer | [SystemName](../primitives.md#systemname) | no (yes) | The name of the system that needs access to the target. Mandatory if the _provider_ is the requester.
cloud | [CloudIdentifier](../primitives.md#cloudidentifier) | no | The cloud of the consumer. Optional, if the consumer is in the Local Cloud.
targetType | [AuthorizationTargetType](../primitives.md#authorizationtargettype) | yes | The type of the target (service definition or event type).
target | [ServiceName](../primitives.md#servicename) or [EventTypeName](../primitives.md#eventtypename) | yes | The name of the target.
scope | [ServiceOperationName](../primitives.md#serviceoperationname) | no | The service operation that the consumer wants to use. Only matters when the target is a service definition.
