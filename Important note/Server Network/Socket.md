[[Important Note Contents]] #Knowlaged-Category #Server-Network

WebSockets are a computer communications protocol that provides a persistent, bidirectional, and full-duplex communication channel over a single TCP connection. Unlike traditional HTTP requests, where the client initiates a request and the server responds, WebSockets allow both the client and the server to send data to each other at any time, making it ideal for real-time applications such as chat applications, online games, or live updates.

WebSockets provide a way for the client and server to communicate in real-time using a **persistent**, **bidirectional connection**. 
## How WebSockets Work
- **Handshake**: The client (e.g., a web browser) initiates a WebSocket connection by sending an HTTP request with an upgrade header to the server. If the server supports WebSockets, it responds with a special handshake response, and the connection is established.
- **Data Transfer**: Once the connection is established, both the client and the server can send data to each other in the form of messages. These messages can be text or binary data, and they are transmitted over the same TCP connection.
- **Connection Closure**: When either the client or the server wants to terminate the connection, it sends a control frame to signal the intent to close the connection.

```merm
sequenceDiagram
    participant Client
    participant Server

    Client->>Server: HTTP GET with Upgrade header
    Server-->>Client: HTTP 101 Switching Protocols
    Note right of Client: WebSocket Connection Established

    Client->>Server: WebSocket Message
    Server->>Client: WebSocket Message
    Client->>Server: WebSocket Message
    Server->>Client: WebSocket Message

    Note right of Client: Real-time, bidirectional communication

    Client->>Server: WebSocket Close frame
    Server-->>Client: WebSocket Close frame
    Note right of Client: WebSocket Connection Closed
```
- This sequence diagram illustrates the following steps:
	1. The client initiates the WebSocket connection by sending an HTTP GET request with an Upgrade header to the server.
	2. The server responds with an HTTP 101 Switching Protocols response, indicating that the WebSocket connection has been established.
	3. Once the connection is established, both the client and the server can send WebSocket messages to each other in real-time, enabling bidirectional communication.
	4. When either the client or the server wants to terminate the connection, they send a WebSocket Close frame, and the connection is closed.

