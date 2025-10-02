# TranslationNegotiationResponse

Field | Type | Description
--- | --- | --- 
bridgeId | [BridgeIdentifier](../primitives.md#bridgeidentifier) | Unique identifier of the translation bridge.
bridgeInterface | [TranslationNegotiationServiceInstance- InterfaceDescriptor](../data-models/translation-negotiation-service-instance-interface-descriptor.md) |Access information for the translation bridge.
tokenExpiresAt | [DateTime](../primitives.md#datetime) | Token expiry date. Only filled if target provider’s operation needs an expirable token.
tokenUsageLimit | [Number](../primitives.md#number) | A number about how many times a token can be used. Only filled if target provider’s operation needs a usage limited token.