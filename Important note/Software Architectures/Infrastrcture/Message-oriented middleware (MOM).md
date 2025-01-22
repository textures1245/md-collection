[[Infrastructure Overview]] [[Micro-service Overview]]

- Software or hardware infrastructure that supports **sending and receiving messages between distributed systems**. It allows different applications, often running on different platforms, to communicate with each other by exchanging messages. Here are some key points about MOM

1. **Asynchronous Communication**: MOM supports asynchronous communication, meaning that the sender and receiver of the message do not need to interact with the message at the same time. This allows for more flexible and decoupled system architectures[1](https://en.wikipedia.org/wiki/Message-oriented_middleware).

2. **Message Brokers**: MOM often uses message brokers to manage the routing, transformation, and delivery of messages. These brokers ensure that messages are delivered to the correct destination, even if the destination is temporarily unavailable[2](https://www.g2.com/articles/message-oriented-middleware).

3. **Loose Coupling**: By using MOM, systems can achieve loose coupling, where components interact with each other without needing to know the details of each other's implementation. This makes it easier to develop, maintain, and scale distributed systems[1](https://en.wikipedia.org/wiki/Message-oriented_middleware).

4. **Fault Tolerance and Reliability**: MOM can provide features like message buffering, which helps in maintaining communication even in the event of network failures or other issues. This enhances the reliability and fault tolerance of the system[3](https://www.ibm.com/think/topics/middleware).

5. **Examples**: Some popular MOM systems include Apache Kafka, RabbitMQ, and IBM MQ. These systems are widely used in various industries to enable robust and scalable communication between distributed applications[2](https://www.g2.com/articles/message-oriented-middleware).