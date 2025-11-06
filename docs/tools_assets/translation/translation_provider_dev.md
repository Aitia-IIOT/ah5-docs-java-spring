# Translation Provider Development

## Interface Translation Provider

The following specifications must be met to develop a compliant [Interface Translation Provider](./translation_providers.md#interface-translation-providers):

| Specifications |     |
| -------------- | --- |
| **InterfaceTranslator** System | [System Description](../../assets/sysd/5_1_0/InterfaceTranslator_sysd.pdf) |
| **interfaceBridgeManagement** Service (provide) | [Service Description](../../assets/sd/5_1_0/interfaceBridgeManagement_sd.pdf), [Interface Design Description](../../api/add-on/interfaceBridgeManagement-generic-http.md) |
| **translationReport** Service (consume) | [Service Description](../../support_systems/translation_manager.md#translationreport), [Interface Design Description](../../api/translationmanager/translation-report-generic-http.md) |
| **dataModelTranslation** Service (consume) | [Service Description](../../assets/sd/5_1_0/dataModelTranslation_sd.pdf), [Interface Design Description](../../api/add-on/dataModelTranslation-generic-http.md) |

### Service security requirements

The interfaceBridgeManagement service can be provided with any of the available [security policies](../../help/service-security.md#security-policies).

### Service registration requirements

As stated in the interfaceBridgeManagement Service Description, the supported interface template names and the translation direction must be included in the service metadata when the system registers its service into the [ServiceRegistry Core System](../../core_systems/service_registry.md).

The key must be **interfaceBridge** and its value must be an object with **form** and **to** keys. The _from_ is always a list of interface template names and _to_ is always one particular interface template name. For example:

```
{
   "interfaceBridge": {
      "from": ["generic_mqtt", "generic_http"],
      "to": "generic_http"
   }
}
```

[Service registration](../../core_systems/service_registry.md#servicediscovery) example:

```
{
   "serviceDefinitionName":"interfaceBridgeManagement",
   "version":"1.0.0",
   "metadata":{
         "interfaceBridge": {
            "from": ["generic_mqtts", "generic_https"],
            "to": "generic_https"
      }
   },
   "interfaces":[
      {
         "templateName":"generic_https",
         "protocol":"https",
         "policy":"CERT_AUTH",
         "properties":{
            "accessAddresses":[
               "192.168.56.116",
               "itb.example.com"
            ],
            "accessPort":8080,
            "basePath":"/translation",
            "operations":{
               "check-targets":{
                  "method":"POST",
                  "path":"/check"
               },
               "initialize-bridge":{
                  "method":"POST",
                  "path":"/init"
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

## Data Model Translation Provider

The following specifications must be met to develop a compliant [Data Model Translation Provider](./translation_providers.md#data-model-translation-providers):

| Specifications |     |
| -------------- | --- |
| **DataModelTranslator** System | [System Description](../../assets/sysd/5_1_0/DataModelTranslator_sysd.pdf) |
| **dataModelTranslation** Service (provide) | [Service Description](../../assets/sd/5_1_0/dataModelTranslation_sd.pdf), [Interface Design Description](../../api/add-on/dataModelTranslation-generic-http.md) |

### Service security requirements

The dataModelTranslation service must be provided with either the [NONE](../../help/service-security.md#none) or [CERT_AUTH](../../help/service-security.md#cert_auth) security policy. When the [TranlsationManager Support System](../../support_systems/translation_manager.md) applies the [CERT_AUTH](../../help/service-security.md#cert_auth) policy, then it looks for dataModelTranslation service instances configured with [CERT_AUTH](../../help/service-security.md#cert_auth) or [NONE](../../help/service-security.md#none). If it applies the [NONE](../../help/service-security.md#none) policy, it looks only for dataModelTranslation service instances configured with [NONE](../../help/service-security.md#none).

### Service registration requirements

As stated in the dataModelTranslation Service Description, the supported data model IDs and the translation direction must be included in the service metadata when the system registers its service into the [ServiceRegistry Core System](../../core_systems/service_registry.md).

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
         "templateName":"generic_https",
         "protocol":"https",
         "policy":"CERT_AUTH",
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