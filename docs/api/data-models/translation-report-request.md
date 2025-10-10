# TranslationReportRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
bridgeId | [TranslationBridgeID](../primitives.md#translationbridgeid) | yes | The unique identifier of the related translation bridge.
timestamp | [DateTime](../primitives.md#datetime) | yes | Event occurs at this time.
state | [TranslationBridgeEventState](../primitives.md#translationbridgeeventstate) | yes | The kind of the event. Determines the new state of the translation bridge.
errorMessage | [String](../primitives.md#string) | no | Error message if the report is about a translation bridge error.
