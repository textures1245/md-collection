[[Go Lectures]] #Go-Basic #Go-Concept 

_Timeouts_ are important for programs that connect to external resources or that otherwise need to bound execution time.

```go
package main

import (
    "fmt"
    "time"
)

func main() {

	// (1)
    c1 := make(chan string, 1) // prevent goroutine leaks in case the channel is `never read` by used channel buffered
    go func() {
        time.Sleep(2 * time.Second)
        c1 <- "result 1"
    }()

	// (2)
    select {
    case res := <-c1:
        fmt.Println(res)
    // this would be `deadlock` if we don't use channel buffer. Since this `c1` channel would not be able to send the value
    case <-time.After(1 * time.Second):
        fmt.Println("timeout 1", res)
    }

	
    c2 := make(chan string, 1)
    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "result 2"
    }()
    select {
    case res := <-c2:
        fmt.Println(res)
    case <-time.After(3 * time.Second):
        fmt.Println("timeout 2")
    }
}
```

- Note (1) that the channel is **buffered**, so the send in the goroutine is nonblocking. This is a common pattern to prevent goroutine leaks in case the channel is `never read`.
- On (2) and (3)  the `select` implementing a timeout. `res := <-c1` awaits the result and `<-time.After` awaits a value to be sent after the timeout which are **1s** and **3s** 

```terminal
timeout 1
result 2
```