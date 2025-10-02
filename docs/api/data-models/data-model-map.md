# DataModelMap

An Object that maps the key _dataModels_ to an Object, in which keys are [ServiceOperationName](../primitives.md#serviceoperationname)s and the values are Objects containing [DataModelIdentifier](../primitives.md#datamodelidentifier)s using keys _input_ and _output_. If an operation has input payload then the identifier of the input data model is assigned to the key _input_. If the operation has response payload then the identifier of the response data model is assigned to the key _output_. If an operation has no input and/or response payload, the appropriate key-value pair must be omitted. If an operation has no payload at all, then that operation can be omitted from the structure.

Should looks like this:

```
dataModels: {
  <operation name>: {
    input: <input data model id>
    output: <output data model id>
  }
}
```
