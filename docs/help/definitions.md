# Definitions

## Primary

### Local Cloud

A preferably private and protected network of loosely coupled collaborating [systems](./definitions.md#microsystem-or-system), that are performing on a well-defined domain (such as a geographically bounded factory or any encapsulated group of automatization tasks).


### Microsystem or System

An identifiable software instance that collaborates within a [Local Cloud](./definitions.md#local-cloud). A system can play a [consumer](./definitions.md#consumer) role, a [provider](./definitions.md#provider) role or both.

### Microservice or Service

An identifiable set of [service-operations](./definitions.md#service-operation) that covers a well-definied scope of activities. 

### Service Operation

An individually identifiable component of a [service](./definitions.md#microservice-or-service), that can be consumed by a collaborating [system](./definitions.md#microsystem-or-system). An operation performs a single physical or virtual action related to its [service](./definitions.md#microservice-or-service) scope, while producing output data and possibly processing input data. 

### Core System

A fundamental [system](#microsystem-or-system) component of a [Local Cloud](#local-cloud) that addresses essential needs. No Local Cloud can be established without at least one Core System.

### Support System

Additional [system](#microsystem-or-system) component to a [Local Cloud](#local-cloud) that addresses general, but not essential needs. 

### Application system

A [micro system](#microsystem-or-system) that joins to a [Local Cloud](#local-cloud) to provide and/or to consume [micro services](#microservice-or-service).

## Supplementary

### Access Token

An experiable reference that points to or holds the access permission details of an authenticated [system](./definitions.md#microsystem-or-system).

### Certificate

A digital certificiate that securely holds the identity of its owner and also can be used to encrypt the data transfer between the communicating parties.

### Consumer 

A [system](./definitions.md#microsystem-or-system) that wants use [services](./definitions.md#microservice-or-service).

### Event

A short notification about a significant occurrence or change in state that is recognized within a [system](./definitions.md#microsystem-or-system) by itself and is boradcasted within the [Local Cloud](./definitions.md#local-cloud). The notification is not holding any other data, than an [event-type], a timestamp, a sender system reference and a small(!) payload if really necessary.

### Event-Type

A textual identifier that refers to the nature of a given [event](./definitions.md#event).

### Identity Info

A self-declaration based identifier that is associated with a [system](./definitions.md#microsystem-or-system) considered as authenticated.

### Identity Token

An experiable reference that points to an identifier that is associated with a [system](./definitions.md#microsystem-or-system) to be authenticated.

### Provider

A [system](./definitions.md#microsystem-or-system) that offers [services](./definitions.md#microservice-or-service).