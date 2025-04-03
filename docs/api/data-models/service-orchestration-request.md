# ServiceOrchestrationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
serviceRequirement | [ServiceOrchestrationRequiremet](../data-models/service-orchestration-requirement.md) | yes | Details of the targeted service.
orchestrationFlags | List<[ServiceOrchestrationFlag](../data-models/service-orchestration-flag.md)> | no | List of orchestration fleg to control the orchestartion process.
qosRequirements | [QoSRequirements](../data-models/qos-requirements.md) | no | Quality of service requirements.
exclusivityDuration | [Name](../primitives.md#number) | no | The interval the service wanted to be exclusive.