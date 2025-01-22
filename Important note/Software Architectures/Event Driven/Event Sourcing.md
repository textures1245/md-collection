[[Event Driven Architecture]]

## Concept

- Event Sourcing is a pattern in which we **store events that happen with new data (entities) as events instead of updating over the old data.** Events are things that happen to that data (state transition). Therefore, every time we create, change, or delete data various Instead of updating the new data over the old data, we add events that occur instead. What we will use to store these events is called an event store, which can be any type of data storage such as a database or file.

## Key Concepts
- **Events are immutable**: it can't be updating over the old event, instead it will append new event (like LinkedList)
- **Events are arranged in chronological order (Event Stream)**: The job of creating these events is a domain object, such as an entity or Aggregate Root (if using Domain-Driven Design), before being stored in the event store through the repository.
  ![](https://miro.medium.com/v2/resize:fit:1260/format:webp/1*-biZnLtjURfiKXjgMvODzA.png)


Once we collect data in the form of Event Sourcing, we can replay this information to be useful in many situations. For example, I can create a set of code as a simple function that takes a list of these events and transforms it into a data structure like the image above, or it can be used to make other data structures such as order reports or data for analytics etc. We can called this as **Projection**


## Projection With CQRS
[[Command Query Response Segregation (CQRS)]]

- Creating this projection is part of creating a read model used for queries in a CQRS pattern that separates the application into command (write) and query (read) parts. Event Sourcing will be implemented on the command side.
	![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*KxIup2ArIz2aB2X41XFjsw.png)

- What should we do? In order for Service B to know that it needs to retrieve new data, If there is an event where 
1. Service A writes data into the Database 
2. Service B wants to read data from the Database, 

	 CQRS will use it together with Event sourcing to stream events, writing to the registered service via Event Stream such as Kafka etc.
	  ![](https://miro.medium.com/v2/resize:fit:1058/format:webp/1*C3goXb1Ey8tc_XXU7Mhzag.png)
	  1. Chat Service has data written to the Database and has a Search Service for searching Chat data. 
	  2. the Search Service that performs the search must have a Data Store for itself such as Elasticsearch 
	  3. Every time a new Event Chat is created or a new change is made, the Event Stream must be entered into the Message Queue to write data. And when finished writing, continue streaming the event to Search Service.