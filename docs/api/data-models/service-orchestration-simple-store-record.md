# ServiceOrchestrationSimpleStoreRecord

Field | Type | Description
--- | --- | ---
id | [UUID](../primitives.md#uuid) | Unique identifier of the store entry.
consumer | [SystemName](../primitives.md#systemname) | Name of the consumer system that the rule applies to.
serviceDefinition | [ServiceName](../primitives.md#servicename) | Name of the service definition that the rule applies to.
serviceInstanceId | [ServiceInstanceId](../primitives.md#serviceinstanceid) | Identifier of the service instance that the rule applies to.
priority | [Number](../primitives.md) | Priority of the rule.
createdBy | [SystemName](../primitives.md#systemname) | The rule was created by this system.
updatedBy | [SystemName](../primitives.md#systemname) | The rule was updated by this system.
createdAt | [DateTime](../primitives.md#datetime) | The rule was created at this timestamp.
updatedAt | [DateTime](../primitives.md#datetime) | The rule was updated at this timestamp.