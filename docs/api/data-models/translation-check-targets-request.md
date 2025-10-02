# TranslationCheckTargetsRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
targetOperation | [ServiceOperationName](../primitives.md#serviceoperationname) | yes | The target service-operation.
targets | List<[TranslationTargetRequest](./translation-target-request.md)> | yes | List of candidate service providers to filter based on translatable interfaces.