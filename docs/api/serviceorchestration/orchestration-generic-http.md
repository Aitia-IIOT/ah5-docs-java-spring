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
POST /serviceregistry/device-registry/register HTTP/1.1
Authorization: Bearer <identity-info>

{
   
}
```

### subscribe

TODO

### unsubscribe

TODO