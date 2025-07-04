# Definitions

## Primary

### Local Cloud

A preferably private and protected network of loosely coupled collaborating [systems](./definitions.md#system), that are performing on a well-defined domain (such as a geographically bounded factory or any encapsulated group of automatization tasks).


### System

An identifiable sowftware instance that collaborates within a [Local Cloud](./definitions.md#local-cloud). A system can play a [consumer](./definitions.md#consumer) role or a [provider](./definitions.md#provider) role or both.

### Service

An identifiable set of [service-operations](./definitions.md#service-operation) that covers a well-definied scope of activity. 

### Service Operation

An individually identifiable component of a [service](./definitions.md#service), that can be consumed by a collaborating [system](./definitions.md#system). An operation performs a single physical or virtual action related to its [service](./definitions.md#service) scope, while producing output data and possibly processing input data. 

## Supplementary

### Access Token

An experiable reference that points to or holds the access permission details of an authenticated [system](./definitions.md#system).

### Certificate

A digital certificiate that securely holds the identity of its owner and also can be used to encrypt the data transfer between the communicating parties.

### Consumer 

A [system](./definitions.md#system) that wants use [services](./definitions.md#service).

### Event

A short notification about a significant occurrence or change in state that is recognized within a [system](./definitions.md#system) by itself and is boradcasted within the [Local Cloud](./definitions.md#local-cloud). The notification is not holding any other data, than an [event-type], a timestamp and a sender system reference.

### Event-Type

A textual identifier that refers to the nature of a given [event](./definitions.md#event).

### Identity Info

A self-declaration based identifier that is associated with a [system](./definitions.md#system) considered as authenticated.

### Identity Token

An experiable reference that points to an identifier that is associated with a [system](./definitions.md#system) to be authenticated.

### Provider

A [system](./definitions.md#system) that offers [services](./definitions.md#service).