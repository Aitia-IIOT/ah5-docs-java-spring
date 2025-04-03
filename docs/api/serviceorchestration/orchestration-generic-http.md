# orchestration IDD
**GENERIC-HTTP & GENERIC-HTTPS**

## Overview

This page describes the GENERIC-HTTP and GENERIC-HTTPS service interface of orchestration, which provides runtime (late) binding between application systems. It’s implemented using protocol, encoding as stated in the
following tables:

**GENERIC-HTTP**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTP | 1.1
Data encryption | N/A | -
Encoding | JSON | RFC 8259
Compression | N/A | -

**GENERIC-HTTPS**

Profile type | type | Version
--- | --- | ---
Transfer protocol | HTTPS | 1.1
Data encryption | TLS | -
Encoding | JSON | RFC 8259
Compression | N/A | -

Hereby the **Interface Design Description** (IDD) is provided to the [orchestration – Service Description](../../assets/sd/5_0_0/orchestration_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### pull

The service operation **request** requires an [identity related header or certfificate](../authentication_policy.md/#http) and a [ServiceOrchestrationRequest](../data-models/service-orchestration-request.md)
JSON encoded body.

```
POST /serviceorchestration/orchestration/pull HTTP/1.1
Authorization: Bearer <identity-info>

{
   "serviceRequirement":{
      "serviceDefinition":"kelvin-info",
      "operations":[
         "query-temperature"
      ],
      "versions":[],
      "alivesAt":"2030-01-01T00:00:00Z",
      "metadataRequirements":[],
      "interfaceTemplateNames":[
         "GENERIC-HTTP"
      ],
      "interfaceAddressTypes":[
         "HOSTNAME",
         "IPV4"
      ],
      "interfacePropertyRequirements":[],
      "securityPolicies":[
         "TOKEN_AUTH"
      ],
      "preferredProviders":[]
   },
   "orchestrationFlags":{
      "MATCHMAKING":"true",
      "ALLOW_TRANSLATION":"true",
      "ONLY_PREFERRED":"false",
      "ONLY_EXCLUSIVE":"false",
      "ALLOW_INTERCLOUD":"false",
      "ONLY_INTERCLOUD":"false"
   },
   "qosRequirements":{
      "max-latency-ms":"10"
   },
   "exclusivityDuration":600
}
```

### subscribe

TODO

### unsubscribe

TODO