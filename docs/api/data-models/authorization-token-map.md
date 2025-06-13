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
_scope_: The [OperationName](../primitives.md#operationname) or `*` if the token in the [`AuthorizationTokenDescriptor`](./authorization-token-descriptor.md) is valid for all the operations:  