[[Go Lectures]] #Go-Core #Go-Basic  #Go-Concept 

**Contents** #Go-Channel-Contents
#Channels 
#Channel-Buffering
#Channels-Synchronization
#Channel-Direction
#Non-Blocking-Channel-Operations
#Closing-Channels
#Range-over-Channels


# Channels 
#Channels
_Channels_ are the pipes that connect concurrent [[Goroutine]]. You can send values into channels from one goroutine and receive those values into another goroutine.

- By default sends and receives block until both the sender and receiver are ready. This property allowed us to wait at the end of our program for the logical executing without having to use any other synchronization.  

```merm

stateDiagram-v2
    state Channel {
        state "Create Channel" as create_channel
        state "Launch Goroutine" as launch_goroutine
        create_channel --> launch_goroutine
    }

    state Goroutine {
        state "Receive value to Channel" as receive
        state "Process Data" as process
        state "Send value to Receiver from Channel" as send
        receive --> process
        process --> send
    }

    launch_goroutine --> receive
    send --> launch_goroutine

    note left of receive
        Blocked/deadlock if no data
        available in channel
        
    end note

    note right of send
        Blocked/deadlock if no receiver
        available for channel
    end note
```

```go
func main() {

    messages := make(chan string) // create go channal

    go func() { messages <- "ping" }() // receive value onto channal (works with go routine)

    msg := <-messages // send value to variable
    fmt.Println(msg)
}
```

```go
package main;
import (
   "fmt"
   "time"
);
func buyGlassesAtSevenEleven() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อแว่น : ที่เซเว่น : เสร็จแล้ว");
}
func buyWatchAtCentral() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อนาฬิกา : ที่เซ็นทรัล : เสร็จแล้ว");
}
func buyFruitAtSiamParagon() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อผลไม้ : ที่สยามพารากอน : เสร็จแล้ว");
}
func buyCarAtToyota() {
   time.Sleep(1 * time.Second);
   fmt.Println("ซื้อรถ : ที่ศุนย์โตโยต้า : เสร็จแล้ว");
}
func sendToMisterA(message chan<- string) {//สร้าง Function (1)
   time.Sleep(1 * time.Second);
   message <- "กำลังส่งของให้ นาย A"; // นำค่าเข้าท่อ Channel (2)
}
func main() {
   start := time.Now(); // เริ่มจับเวลาตอนโปรแกรมทำงาน (3)
   ch := make(chan string); // สร้างท่อ Channel เอาไว้ส่งข้อมูล (4)
   go buyGlassesAtSevenEleven();
   go buyWatchAtCentral();
   go sendToMisterA(ch); // ส่งท่อ ch เข้าไปใน Function (5)
   buyFruitAtSiamParagon();
   buyCarAtToyota();
   messageFromMisterB := <-ch; // ค่าจากท่อ Channel จะออกตรงนี้ (6)
  if messageFromMisterB == "กำลังส่งของให้ นาย A" { // (7)
   fmt.Println("นาย A ได้รับของแล้ว");
   fmt.Println("ใช้เวลาในการ Run ทั้งสิ้น : ",time.Since(start),"วินาที");
  };
}
```

```terminal
ซื้อแว่น : ที่เซเว่น : เสร็จแล้ว  
ซื้อผลไม้ : ที่สยามพารากอน : เสร็จแล้ว  
ซื้อนาฬิกา : ที่เซ็นทรัล : เสร็จแล้ว  
ซื้อรถ : ที่ศุนย์โตโยต้า : เสร็จแล้ว  
นาย A ได้รับของแล้ว  
ใช้เวลาในการ Run ทั้งสิ้น : 2s วินาที
```

# Channel Buffering
#Channel-Buffering #Go-Channel-Contents

By default **channels are** **_unbuffered_**, meaning that they will only accept sends (`chan <-`) if there is a corresponding receive (`<- chan`) ready to receive the sent value. 
- **_Buffered channels_** accept a **limited number of values** (fixed channel size) without a corresponding receiver for those values.
- So this they can send and receive without using **goroutines** since the channel size had fixed so there reserved slot to receive and send their values

```go
package main

import "fmt"

func main() {

    messages := make(chan string, 2) // specific channel size for buffering

// Because this channel is buffered, we can send these values into the channel without a corresponding concurrent receive.
    messages <- "buffered"
    messages <- "channel"

    fmt.Println(<-messages)
    fmt.Println(<-messages)
}
```

```terminal
buffered
channel
```


# Channel Synchronization
#Channels-Synchronization #Go-Channel-Contents 

We can use channels to **synchronize execution across goroutines.** Here’s an example of using a blocking receive to wait for a goroutine to finish. When waiting for **multiple goroutines** to finish, you may prefer to use a [WaitGroup](https://gobyexample.com/waitgroups).

```go
package main

import (
    "fmt"
    "time"
)

func worker(done chan bool) {
    fmt.Print("working...")
    time.Sleep(time.Second)
    fmt.Println("done")

    done <- true
}

func main() {

    done := make(chan bool, 1)
    go worker(done)

	// Block until we receive a notification from the worker on the channel.
    <-done 
}
```

```terminal
working...done 
0x1400008c060
```

# Channel Direction
#Channel-Direction #Go-Channel-Contents 

When using channels as function parameters, you can specify if a channel is meant to only send or receive values. This specificity increases the type-safety of the program.

This `ping` function only accepts a channel for sending values. It would be a compile-time error to try to receive on this channel.

```go
package main

import "fmt"

// accept channel for sending value
func ping(pings chan<- string, msg string) {
    pings <- msg
}

// 1. accept channel that will receiving value 2. channel for sending value 
func pong(pings <-chan string, pongs chan<- string) {
    msg := <-pings
    pongs <- msg
}

func main() {
    pings := make(chan string, 1)
    pongs := make(chan string, 1)
    ping(pings, "passed message")
    pong(pings, pongs)
    fmt.Println(<-pongs)
}
```

# Non-Blocking Channel Operations
#Non-Blocking-Channel-Operations #Go-Channel-Contents 

Basic sends and receives on channels are blocking. However, we can use [[Select]] with a `default` clause to implement _non-blocking_ sends, receives, and even non-blocking multi-way `select`s.

```go
package main

import "fmt"

func main() {
    messages := make(chan string, 1)
    signals := make(chan bool)

	// using `default` to implement non-blocking sending
    select {
    case msg := <-messages:
        fmt.Println("received message", msg)
    default: 
        fmt.Println("no message received")
    }

	// using `default` to implement non-blocking received
	msg := "hi"
	select {
	case messages <- msg:
		fmt.Println("sent message", msg)
		msg = <-messages // sending channel's value off 
	default:
		fmt.Println("no message sent")
	}

	s := true
	select {
	case signals <- s:
		fmt.Println("sent signal", s)
	default:
		fmt.Println("no signal sent")
	}

	// using `default` to implement non-blocking multi-way selects
    select {
    case msg := <-messages:
        fmt.Println("received message", msg)
    case sig := <-signals:
        fmt.Println("received signal", sig)
    default:
        fmt.Println("no activity")
    }
}
```

```terminal
no message received // since the channel doesn't receive any value
sent message hi // the channel receiving value
no signal sent // the channel trying to received the value but it don't have receiver since it not the channel buffer 
no activity // no channels are sending any value
```

# Closing Channels
#Closing-Channels #Go-Channel-Contents 

_Closing_ a channel indicates that **no more values will be sent on it**. This can be useful to communicate **completion to the channel’s receivers**.

```go
package main

import "fmt"

func main() {
    jobs := make(chan int, 5)
    done := make(chan bool)

// goroutine for commnuicated wtih channels
    go func() {
        for {
            j, more := <-jobs
            if more {
                fmt.Println("received job", j)
            } else {
                fmt.Println("received all jobs")
                done <- true
                return
            }
        }
    }()

// received values
    for j := 1; j <= 3; j++ {
        jobs <- j
        fmt.Println("sent job", j)
    }
    close(jobs) // closed job
    fmt.Println("sent all jobs")

    <-done // await worker by using `synchronization` approach 

    _, ok := <-jobs
    fmt.Println("received more jobs:", ok)
}
```

```terminal
sent job 1
received job 1
sent job 2
received job 2
sent job 3
received job 3
sent all jobs
received all jobs
received more jobs: false
```

# Range over Channels
#Range-over-Channels #Go-Channel-Contents 

We can also use this syntax to iterate over values received from a channel. 

```go
package main

import "fmt"

func main() {

    queue := make(chan string, 2)
    queue <- "one"
    queue <- "two"
    close(queue)

    for elem := range queue {
        fmt.Println(elem)
    }
}
```



