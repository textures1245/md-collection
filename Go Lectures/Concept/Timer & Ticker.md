[[Go Lectures]] #Go-Core #Go-Basic #Go-Concept 

**Contents**
#Timer
#Ticker

## **Timer**
#Timer 
Timers represent a single event in the future. You tell the timer how long you want to wait, and it provides a channel that will be notified at that time.

```go
package main

import (
	"fmt"
	"time"
)

func main() {

	timer1 := time.NewTimer(2 * time.Second)

	// take chan from timer and wait for it 2 sec
	<-timer1.C
	fmt.Println("Timer 1 fired")

	// (1)
	timer2 := time.NewTimer(time.Second)
	go func() {
		<-timer2.C
		fmt.Println("Timer 2 fired")
	}()
	stop2 := timer2.Stop() // stop the timer2
	if stop2 {
		fmt.Println("Timer 2 stopped")
	}

	time.Sleep(2 * time.Second)
}
```

```bash
// (2)
Timer 1 fired
Timer 2 stopped
```

1. Note that on (2) you would see that `Timer1` has fired but `Timer2` has stopped before it got fired. Because (1) using [[Goroutine]] that works as **Asynchronous** and then it get stop before the goroutine function got executed.

## Ticker
#Ticker 
Timer are for when you want to do something once in the future - **_tickers_** are for when you want to do something repeatedly at regular intervals.

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    ticker := time.NewTicker(500 * time.Millisecond)
    done := make(chan bool)

// (1) used the similar mechanism to timers by using `selete` then return chan from timer to make the `ticker`.
    go func() {
        for {
            select {
            case <-done:
                return
            case t := <-ticker.C:
                fmt.Println("Tick at", t)
            }
        }
    }()

    time.Sleep(1600 * time.Millisecond)
    ticker.Stop()
    done <- true
    fmt.Println("Ticker stopped")
}
```

```bash
Tick at 2024-03-27 14:06:31.472782 +0700 +07 m=+0.501191043
Tick at 2024-03-27 14:06:31.972863 +0700 +07 m=+1.001289168
Tick at 2024-03-27 14:06:32.472753 +0700 +07 m=+1.501195543
Ticker stopped
```

- Note on (1). Used the similar mechanism to timers by using [[Select]] then return chan from timer to make the `ticker`.