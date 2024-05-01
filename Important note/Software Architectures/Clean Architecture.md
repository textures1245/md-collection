[[Important Note Contents]] #Knowlaged-Category 

Ref: [Clean Architecture In go](https://pkritiotis.io/clean-architecture-in-golang/)

![[Pasted image 20240429135535.png]]
- ## Term of Clean Architecture
	Clean Architecture is a software design concept that isolates change through the _separation of concerns_. Its primary focus is separating business logic from the infrastructure of an application. It does this by dividing software into layers.
	
	In combination with Domain Driver Design concepts, these four circles are often grouped into three main layers:
	- **Domain** → Enterprise business rules
	- **Application** → Application use cases & business rules
	- **Infrastructure** → Interface adapters + Framework & Drivers
		- It is common to merge these two outer circles into one since both depend on application and domain - they don’t have any inter-dependencies
	
	We will describe these three layers in detail in this article. The main idea is the same as in the original diagram. Each layer has well-defined boundaries and dependency rules, represented by the arrows and their direction.
	
	Let’s explore each of these layers in more detail.
## [Domain](https://pkritiotis.io/clean-architecture-in-golang/#domain "Permalink")

The domain layer contains the components of the application that describe the business foundations:

- **Domain Entities & Value Objects**
    - You can think of these as the data structures of the business domain
    - _Entities_ are uniquely identifiable structs
    - _Value Objects_ are structs that are not uniquely identifiable, and they describe the characteristics of an Object.
- **Domain Services**
    - They provide domain functionality using entities and objects
        - i.e., data repository

![Clean Architecture - Domain](https://pkritiotis.io/assets/diagrams/clean-architecture/clean-architecture-domain.png)

### Domain Dependencies[Permalink](https://pkritiotis.io/clean-architecture-in-golang/#domain-dependencies "Permalink")

The domain layer does not have any dependencies on other layers. It is also _only_ used by the application layer (This is not strictly always applied; more on that later in _data contracts_)

## Application[Permalink](https://pkritiotis.io/clean-architecture-in-golang/#application "Permalink")

The application layer exposes all supported use cases of the application to the outside world. It consists of:

- **Business logic/Use Cases**
    - Implementation of business requirements
    - We can implement this with command/query separation. We cover this in our sample application.
- **Application Services**
    - They provide isolated business logic/use cases functionality that is required. This functionality, is expressed by use cases.
    - It can be an interface-only service if it is infrastructure-dependent

![Clean Architecture - Application](https://pkritiotis.io/assets/diagrams/clean-architecture/clean-architecture-application.png)

### Application Dependencies[Permalink](https://pkritiotis.io/clean-architecture-in-golang/#application-dependencies "Permalink")

The application layer code depends only on the domain layer.

## Infrastructure[Permalink](https://pkritiotis.io/clean-architecture-in-golang/#infrastructure "Permalink")

Typically, software applications have the following behavior:

1. Receive input to initiate an operation
2. Interact with infrastructure services to complete a function or produce output.

The entry points from which we receive information (i.e., requests) are called _input ports_. The gateways through which we integrate with external services are called _interface adapters_.

Input Ports and interface adapters depend on frameworks/platforms and external services that are not part of the business or domain logic. For this reason, they belong to this separate layer named _infrastructure_. This layer is sometimes referred to as _Ports & Adapters_.

### Interface Adapters[Permalink](https://pkritiotis.io/clean-architecture-in-golang/#interface-adapters "Permalink")

The interface adapters are responsible for **_implementing_ domain and application services**(interfaces) by **integrating** with specific frameworks/providers.

For example, we can use a SQL provider to implement a domain repository or integrate with an email/SMS provider to implement a Notification service.

### Input ports[Permalink](https://pkritiotis.io/clean-architecture-in-golang/#input-ports "Permalink")

The input ports provide the _entry points_ of the application that receive input from the outside world.

For example, an input port could be an HTTP handler handling synchronous calls or a Kafka consumer handling asynchronous messages.

![Clean Architecture - Infrastructure](https://pkritiotis.io/assets/diagrams/clean-architecture/clean-architecture-infrastructure.png)

