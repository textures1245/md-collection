[[Event Driven Patterns Overview]] 

Ref: 
- [แนวคิดของ CQRS บนโลก Microservice](https://goangle.medium.com/%E0%B9%81%E0%B8%99%E0%B8%A7%E0%B8%84%E0%B8%B4%E0%B8%94%E0%B8%82%E0%B8%AD%E0%B8%87-cqrs-%E0%B8%9A%E0%B8%99%E0%B9%82%E0%B8%A5%E0%B8%81-microservice-b70751358e4c)
- [CQRS](https://medium.com/@chatthanajanethanakarn/cqrs-%E0%B8%89%E0%B8%9A%E0%B8%B1%E0%B8%9A%E0%B8%AD%E0%B9%88%E0%B8%B2%E0%B8%99%E0%B8%9A%E0%B8%99%E0%B8%A3%E0%B8%96%E0%B9%84%E0%B8%9F%E0%B8%9F%E0%B9%89%E0%B8%B2-dbf44e9a2dc1)
- https://github.com/yerinadler/typescript-event-sourcing-sample-app 

In the business industry we found the bottleneck on the modern software architecture as **3 tier architecture** such an API Composition. was became de-facto standard. 

- **3 tier architecture**
	![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*S-8u_rHGZsiT3QDlXPApGw.png)

	but the problem with this kind of architecture is scalability. more usecase complexity, more it will lead into operate business logic bottleneck.
	
	for example. If you analyze carefully, you will find that API Composition is designed to receive and send basic data based on the Synchronous concept, which does not answer the scaling problem. Therefore, someone thought of the Asyncronous principle to allow communication between Microservices don’t have to stop and wait. and prevent the occurrence of Blocking I/O events or being unable to receive load by using Message Broker / Message Streaming to help.
	![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Fxvq33sOKlD0RzbeV_81CA.png)

So CQRS was introduced to solve the problem

## Concept
	Seperate "Command" and "Query" apart. which each part had completly different action

1. **Command**: Update/Create/Delete or mutated data.
2. Query: Request data, can't perform mutate data.

- which this separation It will be separated from the application layer level to the model level (which may be a data model or a rich domain model like DDD, Clean, Onion). This point is what makes it different from writing a service in general. We often use only one set of service + model to operate.
	***CQRS High-level Diagram** ![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*OZYkUjTusFsv0A3bqSycFw.png)
	
	***CQRS with Layered Architecture**![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*uWHQSWdk6TKngwPfzHHPiQ.png)
	The request lifecycle on the command side comes from the client sending a command through the API, and that command is processed by a command handler. This command handler then calls domain objects, such as aggregate roots in the domain layer, to process the business. Various logic before saving these domain objects into the database through the repository. At the same time, there may be events that occur from various commands sent to the event handler that writes the changed data to the data storage on the other side. For queries only, this is the case where we use asynchronous messaging to sync data. Both sides use different types of data storage.
  
	On the query side, there will be a query handler waiting to receive query requests from the API. Then it will bypass logic in the domain layer by calling data access objects such as ORMs to query data. From read database immediately output to client

## CQRS Usage

- If we look into process from separating Command and Query. Optimize in terms of scalability such as asymmetric read/write ratio, optimizing queries with materialized views to reduce aggregate data across tables or collections, including various use cases that may require specialized data storage, such as search that may require Elasticsearch or use for various analytics tasks

	Then we can separate between read / write. Normally, 
	- when reading data, there will already be a query model to meet UI needs. There may be joins across tables / collections, -- 
	- while the write side will focus on writing data. There is a repository to map data from various domain objects into the database, which will naturally be different.
	![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*JDl278gtmc-TwyYBRpXqNA.png)

- But in the advance use case, for example, wanting to use **read storage as a technology that is different from write storage**, for example, write storage may use MySQL but want to support the use case of doing full-text search with Elasticsearch or optimizing query performance on the read side, if it is For this we need a mechanism to sync between write and read storage.

	In this point, we should make System Producer write publish event into Event bus for Consumers can be subscribe which would involve messaging tool such a RabbiMQ or Kafka
	
	*The event handler receives events from the messaging channel and takes these events into a materialized view that is ready to be queried immediately.*![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*95oExSXzx6pA7qn7eD6gjw.png)



