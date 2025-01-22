[[SA Overview]] [[Micro-service Overview]]

Ref: 
- https://medium.com/@seetharamugn/the-complete-guide-to-event-driven-architecture-b25226594227
- https://www.somkiat.cc/summary-event-driven-architecture/

Software design pattern that allows **decoupled applications** to asynchronously publish and subscribe (pub/sub) to events through an event broker (modern [[Message-oriented middleware (MOM)]]). Allows information to flow in real time between applications, microservices, and connected devices as events occur throughout the business.

## Concept
- **Loose coupling of applications with Event Broker**: The event broker is a communication middleman between services. This means that applications and devices ==do not need to know where they are sending information or where the information they are consuming comes from==.

![](https://solace.com/wp-content/uploads/2023/07/eda-patterns-diagram-1536x768.png)

## The Advantages
-  **Improved responsiveness, scalability, and agility in business** : since the pattern allows decoupled applications to asynchronously pub/sub. scalability would be easier and responsiveness would be smoothly because Consumers  do not need to wait the event to happened but they can subscribed from the event that already happened in event broker.
- **Everything happens as soon as possible**, and nothing is waiting on anything else.
- **Don’t have to consider what’s happening downstream,** so you can add service instances to scale.
- **Topic routing and filtering can divide up services quickly and easily**, as in command query responsibility segregation.
- To **add another service**, you can just have it **subscribe to an event and have it generate new events of its own**; there’s no impact on existing services.

## Usecases
- Integrating applications
- Sharing and democratizing data across applications
- Connecting IoT devices for data ingestion and analytics
- Event-enabling microservices

## Key Concepts
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*xddgePyDHOiccwFS.png)

#### 1. Event Broker
- An **event Broker** is a middleware (which can be software, an appliance, or SaaS) that routes events between systems using the publish-subscribe messaging pattern. All applications connect to the event broker, which is responsible for accepting events from senders and delivering them to all systems subscribed to receive them.
  
  It takes good system design and governance to ensure that events end up where they are needed and effective communication between those sending events and those who need to respond. This is where tooling—such as an **event portal**—can help capture, communicate, document, and govern event-driven architecture.

#### 2. Event Portal
- As organizations look to adopt an event-driven architecture, many are finding it difficult to document the design process and understand the impacts of changes to the system. **Event portals** let people design, create, discover, catalog, share, visualize, secure, and manage events and event-driven applications. Event portals serve three primary audiences:
  
1. Architects use an event portal to define, discuss, and review events, data definitions, and application relationships.
2. Developers use an event portal to discover, understand, and reuse events across applications, lines of business, and between external organizations.
3. Data scientists use an event portal to understand event-driven data and discover new insights by combining events.

#### 3. Topics
- Events are tagged with metadata that describes the event, called a “topic.” A topic is a hierarchical text string that describes what’s in the event. Publishers just need to know what topic to send an event to, and the event broker takes care of delivery to systems that need it. Application users can register their interest in events related to a given topic by _subscribing_ to that topic. They can use wildcards to subscribe to a group of topics that have similar topic strings. By using the correct topic taxonomy and subscriptions, you can fulfill two rules of event-driven architecture:

1. A subscriber should subscribe only to the events it needs. The subscription should do the filtering, not the business logic.
2. A publisher should only send an event once, to one topic, and the event broker should distribute it to any number of recipients.

#### 4. Event Mesh
- An event mesh is created and enabled through a network of interconnected event brokers. It’s a configurable and dynamic infrastructure layer for distributing events among decoupled applications, cloud services, and devices by dynamically routing events to any application, no matter where these applications are deployed in the world, in any cloud, on-premises, or IoT environment. Technically speaking, an event mesh is a network of interconnected event brokers that share consumer topic subscription information and route messages amongst themselves so they can be passed along to subscribers.

#### 5. Deferred Execution
- If you’re used to REST-based APIs, the concept of deferred execution can be tricky to comprehend. The essence of event-driven architecture is that when you publish an event, you don’t wait for a response. The event broker “holds” (persists) the event until all interested consumers accept or receive it, which may be sometime later. Acting on the original event may then cause other events to be emitted that are similarly persistent.
  
  So event-driven architecture leads to cascades of events that are temporally and functionally independent of each other but occur in a sequence. All we know is that **event A** will at some point cause something to happen. The execution of the logic-consuming **event A** isn’t necessarily instant; its execution is deferred.

#### 6. Eventual Consistency
- Following on from this idea of deferred execution, where you expect something to happen later but don’t wait for it, is the idea of **eventual consistency**. Since you don’t know when an event will be consumed and you’re not waiting for confirmation, you can’t say with certainty that a given database has fully caught up with everything that needs to happen to it and doesn’t know when that _will_ be the case. If you have multiple stateful entities (database, MDM, ERP), you can’t say they will have exactly the same state; you can’t assume they are consistent. However, for a given object, we know that it will become consistent _eventually._

#### 7. Choreography
- Deferred execution and eventual consistency lead us to the concept of choreography. To coordinate a sequence of actions being taken by different services, you could choose to introduce a master service dedicated to keeping track of all the other services and taking action if there’s an error. This approach, called _orchestration,_ offers a single point of reference when tracing a problem, but also a single point of failure and a bottleneck.
  
  With event-driven architecture, services are relied upon to understand what to do with an incoming event, frequently generating new events. This leads to a “dance” of individual services doing their own things but, when combined, producing an implicitly coordinated response, hence the term choreography.

#### 8. CQRS: Command Query Responsibility Segregation
- A common way of scaling microservices is to separate the service responsible for doing something (a command) from the service responsible for answering queries. Typically, you have to answer many more queries than for an update or insert, so separating responsibilities this way makes scaling the query service easier.
  
  Using event-driven architecture makes this easy since the topic should contain the verb, so you simply create more instances of the query service and have it listen to the topics with the query verb.

## The 6 Principles of Event-Driven Architecture

There we have it, some main principles of event-driven architecture:

1. Use a network of event brokers to make sure the right “things” get the right events.
2. Use topics to make sure you only send once and only receive what you need.
3. Use an event portal to design, document, and govern event-driven architecture across internal and external teams.
4. Use event broker persistence to allow consumers to process events when they’re ready (deferred execution).
5. Remember, this means not everything is up-to-date (eventual consistency).
6. Use topics again to separate out different parts of a service (command query responsibility segregation).