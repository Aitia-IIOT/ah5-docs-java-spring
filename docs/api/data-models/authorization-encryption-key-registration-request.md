# AuthorizationEncryptionKeyRegistrationRequest

Field | Type | Mandatory | Description
--- | --- | --- | ---
key | [String](../primitives.md) | yes | A secret key.
algorithm | [EncryptionAlgorithmName](../primitives.md#encryptionalgorithmname) | no | Algorithm identifier. Default is `AES/ECB/PKCS5Padding`.