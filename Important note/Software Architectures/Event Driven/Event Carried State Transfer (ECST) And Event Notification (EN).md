[[Event Driven Patterns Overview]]

Ref 
- https://medium.com/swlh/event-notification-vs-event-carried-state-transfer-2e4fdf8f6662
- https://martinfowler.com/articles/201701-event-driven.html


![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*hZ4gInYIVuO89DKrN61TtQ.png)
From the core concept of [[Event Driven Architecture]], we can capture the concept that `events` are past tense that can be immutable and pub/sub from `Producer` and `Consumer` thought middleware named `Broker`.  in this section we would talking about the basic patterns that usually come on this architecture with pro and con for each pattern.

## Event Notification (EN)  

The pattern that we used the most in EDA. Happens when a system sends **event messages** to notify other systems of a change in its domain. But they are key concepts that it different between among of others
- the source system doesn't really care much about the response.
- Often it does or doesn't expect any answer at all.
- it's indirect. There would be a marked separation between the logic flow that sends the event and any logic flow that responds to some reaction to that event.

For example, News Subscribing System.
1. When Producers created news, their can be publisher to Broker without knowing who Consumers they should be send to. Reducing the coupling and data sizing.
2. Instead Consumers will be subscribe/listen from that events. to get those key identify information, and query useful information that be later used.

This is useful if we want 
1. data/event flowing imply a low level of coupling. 
3. Reducing sizing of sending, since those data are key identify to truth data
4. Up-to-date data

#### Disadvantages
But It can become problematic, however, if there really is a logical flow that runs over various event notifications. The problem is that it can be hard to see such a flow as it's not explicit in any program text. since they not synchronous and event that store in Broker are only key identify that need to be query to truth data  

A simple example of this trap is when an event is used as a passive-aggressive command. This happens when the source system expects the recipient to carry out an action, and ought to use a command message to show that intention, but styles the message as an event instead.

## Event Carried State Transfer (ECST)

If EN pattern are became the problematic to scaling, this pattern came to solve one of these problems. 

ECST works by using events to transfer data and state between services. When a **service needs to access data or state that is managed by another service**, it sends a request event to that service. Allows services to operate independently and asynchronously, without the need for direct communication or shared storage.
1. The responding service then sends a response event that contains the requested data or state. 
2. allowing the requesting service to update its local copy of the data (state management). By using events to transfer data. 

This pattern shows up when you want to update clients of a system in such a way that they don't need to contact the source system in order to do further work. A customer management system might fire off events whenever a customer changes their details (such as an address) with events that contain details of the data that changed. A recipient can then update it's own copy of customer data with the changes, so that it never needs to talk to the main customer system in order to do its work in the future.

### **Key Difference**

| Feature          | Event Notification (EN)                 | Event-Carried State Transfer (ECST) |
| ---------------- | --------------------------------------- | ----------------------------------- |
| **Data Content** | Minimal (key or ID)                     | Complete (all required data)        |
| **Coupling**     | High (consumer depends on producer)     | Low (consumer gets all it needs)    |
| **Performance**  | Slower (extra calls for data)           | Faster (no extra calls)             |
| **Message Size** | Small                                   | Larger                              |
| **Flexibility**  | Higher (consumers decide what to fetch) | Lower (data is fixed in the event)  |
