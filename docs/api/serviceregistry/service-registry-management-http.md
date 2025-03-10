# service-registry-management GENERIC-HTTP

## Overview
This page describes the service-registry-management service, which enables systems (with operator role or proper permissions) to handle (register, update, revoke, lookup) devices, systems, service instances, service definitions and interface templates in bulk. An example of this interaction is that an operator uses the Management Tool to register interface templates, systems, and service instances manually. The interfaces are implemented using protocol, encoding as stated in the following tables:

## Interface Description

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

