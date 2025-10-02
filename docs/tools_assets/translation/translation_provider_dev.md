# Translation Provider Development

## Interface Translation Provider

The following specifications must be met to develop a compliant [Interface Translation Provider](./translation_providers.md#interface-translation-providers):

| Specifications |     |
| -------------- | --- |
| **InterfaceTranslator** System | [System Description](../../assets/sysd/5_1_0/InterfaceTranslator_sysd.pdf) |
| **interfaceBridgeManagement** Service | [Service Description](../../assets/sd/5_1_0/interfaceBridgeManagement_sd.pdf), [Interface Design Description](../../api/add-on/interfaceBridgeManagement-generic-http.md) |

## Data Model Translation Provider

The following specifications must be met to develop a compliant [Data Model Translation Provider](./translation_providers.md#data-model-translation-providers):

| Specifications |     |
| -------------- | --- |
| **DataModelTranslator** System | [System Description](../../assets/sysd/5_1_0/DataModelTranslator_sysd.pdf) |
| **dataModelTranslation** Service | [Service Description](../../assets/sd/5_1_0/dataModelTranslation_sd.pdf), [Interface Design Description](../../api/add-on/dataModelTranslation-generic-http.md) |

### Service registration requirements

As stated in the dataModelTranslation Service Description, the supported data model IDs and the translation direction must be inclued in the service metadata when the system registers its service into the [ServiceRegistry Core System](../../core_systems/service_registry.md).

The key must be **dataModelIds** and its value must be an array that contains other arrays. Each embedded array has exactly two elements which specify an input data model and an output data model in that order. For example:

```
"dataModelIds": [["A", "B"], ["A", "C"]]
```

If a service instance supports two-way translation, its dataModelIds field must contain both
pair. For example:

```
"dataModelIds": [["A", "B"], ["B", "A"]]
```

[Service registration](../../core_systems/service_registry.md#servicediscovery) example:

```
{
   "serviceDefinitionName":"dataModelTranslation",
   "version":"1.0.0",
   "metadata":{
      "dataModelIds":[
         [
            "A",
            "B"
         ],
         [
            "C",
            "B"
         ]
      ]
   },
   "interfaces":[
      {
         "templateName":"generic_http",
         "protocol":"http",
         "policy":"NONE",
         "properties":{
            "accessAddresses":[
               "192.168.56.116",
               "dmt.example.com"
            ],
            "accessPort":8080,
            "basePath":"/translation",
            "operations":{
               "init-translation":{
                  "method":"POST",
                  "path":"/init"
               },
               "get-translation-result":{
                  "method":"GET",
                  "path":"/result"
               },
               "abort-translation":{
                  "method":"DELETE",
                  "path":"/abort"
               }
            }
         }
      }
   ]
}
```