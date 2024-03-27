[[Go Lectures]] #Go-Core #Go-Concept 

**Content**
#Rate-limit
#Atomic-Counters
#Mutexes

## Rate Limiting
#Rate-limit 
[_Rate limiting_](https://en.wikipedia.org/wiki/Rate_limiting) is an important mechanism for controlling resource utilization and maintaining quality of service. Go elegantly supports rate limiting with [[Goroutine]], [[Go Channels]], and [[Timer & Ticker]].

```go
package main

import (
    "fmt"
    "time"
)

func main() {

// created req chan for received incoming reqs 
    requests := make(chan int, 5)
    for i := 1; i <= 5; i++ {
        requests <- i
    }
    close(requests)

    limiter := time.Tick(200 * time.Millisecond) // set limiter = 200 mills

// limit each request followed the limiter
    for req := range requests {
        <-limiter
        fmt.Println("request", req, time.Now())
    }

// (1)
    burstyLimiter := make(chan time.Time, 3)

// Fill up the channel to represent allowed bursting.
    for i := 0; i < 3; i++ {
        burstyLimiter <- time.Now()
    }

// using goroutine to tick every 200 mills to fill up the channal if it can.
    go func() {
        for t := range time.Tick(200 * time.Millisecond) {
            burstyLimiter <- t
        }
    }()

// created new bursty req chan for 5 up incoming reqs
    burstyRequests := make(chan int, 5)
    for i := 1; i <= 5; i++ {
        burstyRequests <- i
    }
    close(burstyRequests)
    for req := range burstyRequests {
        <-burstyLimiter // limit reqs followed bursty limiter
        fmt.Println("request", req, time.Now())
    }
}
```

- note on (1). When allow short bursts of requests in our rate limiting scheme while preserving the overall rate limit. We can accomplish this by buffering our limiter channel. Here we allow to short bursts of 3 incoming requests before preserving the overall rate limit.

```terminal
// batch 1
request 1 2024-03-27 15:35:17.067136 +0700 +07 m=+0.201275835
request 2 2024-03-27 15:35:17.267128 +0700 +07 m=+0.401271543
request 3 2024-03-27 15:35:17.467129 +0700 +07 m=+0.601276501
request 4 2024-03-27 15:35:17.667083 +0700 +07 m=+0.801235126
request 5 2024-03-27 15:35:17.867115 +0700 +07 m=+1.001270960

// batch 2
request 1 2024-03-27 15:35:17.867248 +0700 +07 m=+1.001404460
request 2 2024-03-27 15:35:17.86726 +0700 +07 m=+1.001415960
request 3 2024-03-27 15:35:17.867264 +0700 +07 m=+1.001420251
request 4 2024-03-27 15:35:18.067307 +0700 +07 m=+1.201467668
request 5 2024-03-27 15:35:18.267284 +0700 +07 m=+1.401448626
```

- on `batch 1`: requests handled once every ~200 milliseconds as desired.
- on `batch 2`: requests we serve the first 3 immediately because of the **burstable rate limiting**, then serve the **remaining 2 with ~200ms delays each**  (from using **ticker**)

## [Atomic Counters](https://medium.com/@hatronix/go-go-lockless-techniques-for-managing-state-and-synchronization-5398370c379b)
#Atomic-Counters 

The primary mechanism for managing state in Go is communication over channels. We saw this for example with #Worker-Pools ([[Concurrently]]). 

And now we have Atomic counter which designed to be **executed or merging state without interruption and without interference from other concurrent operations**. Ensure that certain operations on shared variables are performed atomically.
- They are executed as a single, indivisible unit.
- They are not subject to interference or data races from other goroutines or threads.

1. Load: The atomic.Load* functions are used to atomically read the value of a variable. 
	1. For example, atomic.LoadInt32 is used to atomically load the value of an int32 variable.
2. Store: The atomic.Store* used set the value of a variable. 
	1. For example, atomic.StoreInt32 is used to atomically set the value of an int32 variable.
3. Add and Subtract: The atomic.Add* and atomic.Sub* functions are used to automically increment or decrement the value of a variable.

```go
package main

import (
	"fmt"
	"sync/atomic"
	"time"
)

func main() {
	var counter int32

	// Create a goroutine to increment the counter.
	go func() {
		for i := 0; i < 5; i++ {
			atomic.AddInt32(&counter, 1)
			fmt.Printf("Incremented: %d\n", atomic.LoadInt32(&counter))
			time.Sleep(time.Millisecond)
		}
	}()

	// Create a goroutine to decrement the counter.
	go func() {
		for i := 0; i < 5; i++ {
			atomic.AddInt32(&counter, -1)
			fmt.Printf("Decremented: %d\n", atomic.LoadInt32(&counter))
			time.Sleep(time.Millisecond)
		}
	}()

	// Wait for the goroutines to finish.
	time.Sleep(2 * time.Second)

// Using `Load` it’s safe to atomically read a value even while other goroutines are (atomically) updating it.
	fmt.Printf("Final Value: %d\n", atomic.LoadInt32(&counter))
}
```

```terminal
Decremented: 0
Incremented: 1
Incremented: 1
Decremented: 0
Decremented: -1
Incremented: 0
Decremented: -1
Incremented: 0
Incremented: 1
Decremented: 0
Final Value: 0
```

## Mutexes
#Mutexes 

For more complex state we can use a [_mutex_](https://en.wikipedia.org/wiki/Mutual_exclusion) to safely access data across multiple goroutines.

```go
package main

import (
    "fmt"
    "sync"
)

// (1)
type Container struct {
    mu       sync.Mutex
    counters map[string]int
}

func (c *Container) inc(name string) {

// (2)
	c.mu.Lock()
    defer c.mu.Unlock() 
    c.counters[name]++
}

func main() {
    c := Container{

        counters: map[string]int{"a": 0, "b": 0},
    }

    var wg sync.WaitGroup

    doIncrement := func(name string, n int) {
        for i := 0; i < n; i++ {
            c.inc(name)
        }
        wg.Done()
    }

// Run several goroutines concurrently; note that they all access the same `Container`, and two of them access the same counter.
    wg.Add(3)
    go doIncrement("a", 10000)
    go doIncrement("a", 10000)
    go doIncrement("b", 10000)

    wg.Wait()
    fmt.Println(c.counters)
}
```

1. add **mutex** for **synchronize access**. note that **mutexes must not be copied,** so if this struct is passed around, it should be **done by pointer**.
2. **Lock the** **mutex** before accessing counters; **unlock it at the end of the function** using a #defer statement.
```terminal
map[a:20000 b:10000]
```
