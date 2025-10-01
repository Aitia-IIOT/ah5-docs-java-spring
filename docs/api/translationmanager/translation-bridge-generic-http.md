# translationBridge IDD
**generic_http & generic_https**

## Overview

This page describes the [generic_http](../communication-profiles/generic-http-template.md) and [generic_https](../communication-profiles/generic-https-template.md) service interface of translationBridge, which enables to find providers whose service
operation is accessible by the requester via a translation bridge using the currently available translators. The service also allows to build or abort such bridges. o enable other systems to use, to consume it, this service needs to be offered through the ServiceRegistry.

Hereby the **Interface Design Description** (IDD) is provided to the [translationBridge – Service Description](../../assets/sd/5_1_0/translation-bridge_sd.pdf). For further details about how this service is meant to be used, please consult that document.

## Interface Description

### discovery

The service operation **request** requires an [identity related header or certificate](../authentication_policy.md/#http) and an [TranslationDiscoveryRequest](../data-models/translation-discovery-request.md)
JSON encoded body.

TODO



