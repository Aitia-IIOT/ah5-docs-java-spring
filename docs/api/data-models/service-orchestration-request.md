# ServiceOrchestrationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
serviceRequirement | [ServiceOrchestrationRequiremet](../data-models/service-orchestration-requirement.md) | yes | Details of the targeted service.
orchestrationFlags | List<[ServiceOrchestrationFlag](../data-models/service-orchestration-flag.md)> | no | List of orchestration flags to control the orchestration process. (**simple-store** strategy supports only `MATCHMAKING` & `ONLY_PREFERRED`)
qualityRequirements | List<[QoSRequirement](../data-models/qos-requirement.md)> | no | Service quality requirements in order of priority. (not supported by **simple-store** strategy)
exclusivityDuration | [Number](../primitives.md#number) | no | The interval the service wanted to be exclusive. (not supported by **simple-store** strategy)