[[Go Lectures]] #Go-Core #Go-Concept 

**Content** #Go-Resource-Management-Cotents
#Rate-limit
#Atomic-Counters
#Mutexes
#Stateful-Goroutines

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

## Stateful Goroutines
#Stateful-Goroutines

We can use the built-in synchronization features of goroutines and channels to achieve the **shared state across multiple goroutines**.. This channel-based approach aligns with Go’s ideas of **sharing memory by communicating and having each piece of data owned by exactly 1 goroutine.**

```merm
stateDiagram-v2
    state main {
        state "Create Channels" as create_channels
        create_channels --> "Spawn State Goroutine"
        create_channels --> "Spawn Reader Goroutines"
        create_channels --> "Spawn Writer Goroutines"
    }

    state StateGoroutine {
        state "Select" as select
        select --> "Handle Read" : read_op
        "Handle Read" --> select : "Send Response"
        select --> "Handle Write" : write_op
        "Handle Write" --> select : "Send Response"
    }

    state ReaderGoroutine {
        state "Create Read Op" as create_read_op
        create_read_op --> "Send Read Op"
        "Send Read Op" --> "Receive Response"
        "Receive Response" --> create_read_op
    }

    state WriterGoroutine {
        state "Create Write Op" as create_write_op
        create_write_op --> "Send Write Op"
        "Send Write Op" --> "Receive Response"
        "Receive Response" --> create_write_op
    }

	[*] --> "Spawn State Goroutine"
    "Spawn State Goroutine" --> select
    "Spawn Reader Goroutines" --> create_read_op
    "Spawn Writer Goroutines" --> create_write_op

    "Send Read Op" --> select : read_op
    select --> "Receive Response" : "Send Response"
    "Send Write Op" --> select : write_op
    select --> "Receive Response" : "Send Response"

    note right of select
        Handles read and write operations
        by modifying the state map
    end note

    note left of create_read_op
        Constructs a readOp with a
        random key and response channel
    end note

    note left of create_write_op
        Constructs a writeOp with a random
        key, value, and response channel
    end note
```

```go
package main

import (
	"fmt"
	"math/rand"
	"sync/atomic"
	"time"
)

// (1)
type readOp struct {
	key  int
	resp chan int
}
type writeOp struct {
	key  int
	val  int
	resp chan bool
}

func main() {

	// set variable to count how many operations we perform.
	var readOps uint64
	var writeOps uint64

	// used by other goroutines to issue read and write requests, respectively.
	reads := make(chan readOp)
	writes := make(chan writeOp)


// (2)
	go func() {
		var state = make(map[int]int) // create state history
		for {
			select {
			case read := <-reads:
				read.resp <- state[read.key]
			case write := <-writes:
				state[write.key] = write.val
				write.resp <- true
			}
		}
	}()

// (3)
	for r := 0; r < 100; r++ {
		go func() {
			for {
				read := readOp{
					key:  rand.Intn(5),
					resp: make(chan int)}
				reads <- read
				<-read.resp
				atomic.AddUint64(&readOps, 1)
				time.Sleep(time.Millisecond)
			}
		}()
	}

	for w := 0; w < 10; w++ {
		go func() {
			for {
				write := writeOp{
					key:  rand.Intn(5),
					val:  rand.Intn(100),
					resp: make(chan bool)}
				writes <- write
				<-write.resp
				atomic.AddUint64(&writeOps, 1)
				time.Sleep(time.Millisecond)
			}
		}()
	}

	time.Sleep(time.Second) // wait for the goroutines to finished thier work

	readOpsFinal := atomic.LoadUint64(&readOps)
	fmt.Println("readOps:", readOpsFinal)
	writeOpsFinal := atomic.LoadUint64(&writeOps)
	fmt.Println("writeOps:", writeOpsFinal)
}

```

1. State will be owned by a single goroutine. 
	1. This will guarantee that the data is never **corrupted** **with concurrent access**
	2. **other goroutines** will **send messages to the owning goroutine and receive corresponding replies.**
	3. These `readOp` and `writeOp` structs encapsulate those requests and a way for the owning goroutine to respond.
2. This goroutine repeatedly selects on the `reads` and `writes` channels, responding to requests as they arrive. 
	1. A response is executed by first performing the requested operation and then sending a value on the response channel `resp` to indicate success (and the desired value in the case of `reads`).
3. Start goroutines to issue `reads` and `writes` to the state-owning goroutine via the `reads` and `writes` channel.
	1. Each read requires constructing a `readOp`, sending it over the `reads` channel, and then receiving the result over the provided `resp` channel.
	2.  Each write requires constructing a `writeOp`, sending it over the `writes` channel, and then receiving the result over the provided `resp` channel.

```terminal
// around 80k
readOps: 71708
writeOps: 7177
```

- For this particular case the goroutine-based approach was a bit more involved than the mutex-based one. It might be useful in certain cases though, for example where you have other channels involved or when managing multiple such mutexes would be error-prone. 


```merm
flowchart TD
    start([Start]) --> define_state[Define a shared state]
    define_state --> define_mutex[Define a mutex to protect the state]

    define_mutex --> access_state{Access shared state?}
    access_state --Yes--> lock_mutex[Lock the mutex]
    lock_mutex --> read_or_write{Read or Write?}

    read_or_write --Read--> read_state[Read the state]
    read_state --> unlock_mutex[Unlock the mutex]
    unlock_mutex --> process_read[Process read data]

    read_or_write --Write--> modify_state[Modify the state]
    modify_state --> unlock_mutex

    unlock_mutex --> access_state

    access_state -- No --> End([End])

    process_read --> access_state

```
-  Using a mutex ensures that only one goroutine can access the shared state at a time, preventing data races and corruption. 
- So, you should use whichever approach feels most natural, especially with respect to understanding the correctness of your program.