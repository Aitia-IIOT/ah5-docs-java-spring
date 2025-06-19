# AuthorizationTokenMap

An Object which maps [String](../primitives.md#string) `scope`* and [`AuthorizationTokenDescriptor`](./authorization-token-descriptor.md) [KeyValuePairs](../primitives.md#keyvaluepair) to [SecurityPolicy](../primitives.md#securitypolicy) keys. Example:

```
{
    "<security-policy-a>": {
        "<scope>": {...}
    },
    "<security-policy-b>": {
        "<scope-a>": {...},
        "<scope-b>": {...}
    }
}
```
*_scope_: The [ServiceOperationName](../primitives.md#serviceoperationname) or [ServiceName](../primitives.md#servicename) if the token in the [`AuthorizationTokenDescriptor`](./authorization-token-descriptor.md) is valid for all the operations:  