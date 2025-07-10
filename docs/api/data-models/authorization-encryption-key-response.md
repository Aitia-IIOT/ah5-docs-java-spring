# AuthorizationEncryptionKeyResponse

Field | Type | Description
--- | --- | ---
systemName | [SystemName](../primitives.md#systemname) | Name of the associated system.
rawKey | [String](../primitives.md#string) | The raw string key.
algorithm | [EncryptionAlgorithmName](../primitives.md#encryptionalgorithmname) | Name of the ecnryption algorithm.
keyAdditive | [String](../primitives.md#string) | Any string addition what the defined algorithm is using, if any.
createdAt | [DateTime](../primitives.md#datetime) | The encryprtion key was registered at this timestamp.