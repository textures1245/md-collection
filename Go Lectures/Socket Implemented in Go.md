[[Important Note Contents]] [[Go Lectures]] #Server-Network 

**Contents**
#Socket-In-Go-Concept
#fasthttp-Websocket-Implemented

Most programming languages provide the necessary library support for socket programming. The API supported by the library is the network standard for TCP/IP. Which go had `net` package for support bidirectional connection.

## The `net` package 
#Socket-In-Go-Concept
In Go we can used the **net** package provides the necessary APIs to implement socket communication between two of the topmost TCP/IP layers: _application_ and _transport_. The **net** package provides an interface for network I/O, TCP/IP, UDP, domain name resolution, and Unix domain sockets. This package also enables low-level access to network primitives. Basic interfaces for communication are provided by functions like **Dial**, **Listen**, and **Accept** and the associated **Conn** and **Listener** interfaces. 

## Client Socket Basics in Go

As mentioned, a socket is responsible for establishing a connection between the endpoints of two hosts. The basic operation for the client can be discreetly laid out as follows:

- Establish connection to remote host
- Check if any connection error has occurred or not
- Send and receive bytes of information.
- Finally, close the connection

Here is an example of how to use a socket in Go. The client simply establishes a connection with the server, sends some bytes of information, and receives some data from the server:

```go
package main
import (
        "fmt"
        "net"
)
const (
        SERVER_HOST = "localhost"
        SERVER_PORT = "9988"
        SERVER_TYPE = "tcp"
)
func main() {
        //establish connection
        connection, err := net.Dial(SERVER_TYPE, SERVER_HOST+":"+SERVER_PORT)
        if err != nil {
                panic(err)
        }
        ///send some data
        _, err = connection.Write([]byte("Hello Server! Greetings."))
        buffer := make([]byte, 1024)
        mLen, err := connection.Read(buffer)
        if err != nil {
                fmt.Println("Error reading:", err.Error())
        }
        fmt.Println("Received: ", string(buffer[:mLen]))
        defer connection.Close()
}
```


## Server Socket Basics in Go

The socket server waits for incoming calls and responds accordingly. A server is bound by a port and listens to the incoming TCP connections. As the client socket attempts to connect to the port, the server wakes up to negotiate the connection by opening sockets between two hosts. It is this socket between the two forms that acts as the interface for communication and information exchange. In short, what the server socket does is:

- Create a socket on a specific port
- Listen to any attempt of connection to that port
- If the connection is successful, communication can begin between client and server.
- The communication, however, should be according to the agreed protocol
- Continue listening to the port
- Once the connection is closed, the server stops listening and exits.

Here is a server example with reference to the client program code above, written in Golang:
```go
package main
import (
        "fmt"
        "net"
        "os"
)
const (
        SERVER_HOST = "localhost"
        SERVER_PORT = "9988"
        SERVER_TYPE = "tcp"
)
func main() {
        fmt.Println("Server Running...")
        server, err := net.Listen(SERVER_TYPE, SERVER_HOST+":"+SERVER_PORT)
        if err != nil {
                fmt.Println("Error listening:", err.Error())
                os.Exit(1)
        }
        defer server.Close()
        fmt.Println("Listening on " + SERVER_HOST + ":" + SERVER_PORT)
        fmt.Println("Waiting for client...")
        for {
                connection, err := server.Accept()
                if err != nil {
                        fmt.Println("Error accepting: ", err.Error())
                        os.Exit(1)
                }
                fmt.Println("client connected")
                go processClient(connection)
        }
}
func processClient(connection net.Conn) {
        buffer := make([]byte, 1024)
        mLen, err := connection.Read(buffer)
        if err != nil {
                fmt.Println("Error reading:", err.Error())
        }
        fmt.Println("Received: ", string(buffer[:mLen]))
        _, err = connection.Write([]byte("Thanks! Got your message:" + string(buffer[:mLen])))
        connection.Close()
}
```
- Here, the server is set using the **Listen** function, which takes the host port and the type of network as its parameters. The typical network types are: **tcp**, **tcp4**, **tcp6**, **unix**, or **unix packet**. If the host part of the parameter is left unspecified, then the function listens on all available unicast and anycast IP addresses of the local system. Once the server is created, it waits and listens to the port for any incoming communication from any client. As we have seen in the client code above, any messages are sent to – or received from – the client using **Write** and **Read** functions, respectively.
## Fasthttp Websocket
#fasthttp-Websocket-Implemented 

```merm
classDiagram
    class WebSocketConnection {
        +Conn *fasthttpwsConnector.DefalwsClient
        +Upgrade(ctx *fasthttp.RequestCtx) error
        +WriteMessage(messageType int, data []byte) error
        +ReadMessage() (messageType int, data []byte, err error)
        +Close() error
    }

    class WebSocketServer {
        +Upgrader fasthttpwsConnector.DefalwsUpgrader
        +Handler(ctx *fasthttp.RequestCtx)
        -handleWebSocket(conn *WebSocketConnection)
    }

    WebSocketServer --* WebSocketConnection
    fasthttpwsConnectorDefalwsUpgrader ..> WebSocketServer
    fasthttpwsConnectorDefalwsClient ..> WebSocketConnection

    note "The Fasthttp WebSocket package provides\na convenient way to work with WebSockets\nin Go using the FastHTTP web server."
```
- This class diagram illustrates how WebSockets can be adapted in Go using the `Fasthttp WebSocket` package:
	- The `WebSocketConnection` class represents a WebSocket connection. It encapsulates the underlying FastHTTP WebSocket connection (`fasthttpwsConnector.DefalwsClient`) and provides methods for upgrading the HTTP connection to a WebSocket connection (`Upgrade`), sending and receiving WebSocket messages (`WriteMessage` and `ReadMessage`), and closing the connection (`Close`).
	- The `WebSocketServer` class represents the server-side component that handles WebSocket connections. It has an instance of `fasthttpwsConnector.DefalwsUpgrader` for upgrading HTTP connections to WebSocket connections. The `Handler` method is responsible for handling incoming requests and upgrading the connection if necessary. The `handleWebSocket` method (not shown) would handle the logic for processing WebSocket messages received from clients.
	- The `WebSocketServer` class has a composition relationship with the `WebSocketConnection` class, meaning that it creates and manages instances of `WebSocketConnection` to handle individual WebSocket connections.
	- The `fasthttpwsConnector.DefalwsUpgrader` and `fasthttpwsConnector.DefalwsClient` classes are provided by the `Fasthttp WebSocket` package and are used by the `WebSocketServer` and `WebSocketConnection` classes, respectively, to implement WebSocket functionality.