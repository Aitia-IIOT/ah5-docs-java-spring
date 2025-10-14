# TranslationQueryRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
pagination | [PageRequest](../data-models/page-request.md) | no | Paging information about the queried translation bridges.
bridgeIds | List<[TranslationBridgeID](../primitives.md#translationbridgeid)> | no | Requester is looking for translation bridges with any of the specified identifiers.
creators | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for translation bridges that are created by any of the specified systems.
statuses | List<[TranslationBridgeStatus](../primitives.md#translationbridgestatus)> | no | Requester is looking for translation bridges with any of the specified statuses.
consumers | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for translation bridges that are created for any of the specified systems.
providers | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for translation bridges where the target system is in the specified list.
serviceDefinitions | List<[ServiceName](../primitives.md#servicename)> | no | Requester is looking for translation bridges with any of the specified names as target service definition.
interfaceTranslators | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for translation bridges where the used interface translator is in the specified list.
dataModelTranslators | List<[SystemName](../primitives.md#systemname)> | no | Requester is looking for translation bridges where the input or result data model translator is in the specified list.
creationFrom | [DateTime](../primitives.md#datetime) | no | Requester is looking for translation bridges that were created after the specified moment.
creationTo | [DateTime](../primitives.md#datetime) | no | Requester is looking for translation bridges that were created before the specified moment.
alivesFrom | [DateTime](../primitives.md#datetime) | no | Requester is looking for translation bridges that were last used after the specified moment.
alivesTo | [DateTime](../primitives.md#datetime) | no |Requester is looking for translation bridges that were last used before the specified moment.
minUsage | [Number](../primitives.md#number) | no | Requester is looking for translation bridges were used _minUsage_ times at least.
maxUsage | [Number](../primitives.md#number) | no | Requester is looking for translation bridges were used maxUsage times at the most. 