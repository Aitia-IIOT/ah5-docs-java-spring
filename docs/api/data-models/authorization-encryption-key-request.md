# AuthorizationEncryptionKeyRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
systemName | [SystemName](../primitives.md#systemname) | no | Name of the associated system.
key | [String](../primitives.md) | yes | A secret key.
algorithm | [EncryptionAlgorithmName](../primitives.md#encryptionalgorithmname) | no | Algorithm identifier. Default is `AES/ECB/PKCS5Padding`.