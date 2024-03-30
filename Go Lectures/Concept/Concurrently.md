[[Go Lectures]] #Go-Core #Go-Basic #Go-Concept 

**Contents**
#Worker-Pools
#WaitGroup
#Atomic-Counters
#Mutexes 
## Worker Pools
#Worker-Pools
Essential for managing concurrent tasks efficiently in the backend. Is the way how collection of goroutines that are utilized to execute tasks [concurrently](https://web.mit.edu/6.005/www/fa14/classes/17-concurrency/). Designed to manage the execution of tasks efficiently by limiting the number of goroutines created

```merm
stateDiagram-v2
    state "Job Queue" as queue
    state WorkerPool {
        state "Idle Worker" as idle
        state "Working" as working
        idle --> working : "Get Job from Queue"
        working --> idle : "Job Complete"
    }

    queue --> idle : "Dispatch Job"

    note left of idle
        Workers are initially idle
        and waiting for jobs
    end note


    state MainGoroutine {
        state "Create Workers" as create_workers
        state "Submit Jobs" as submit_jobs
        create_workers --> idle : "Initialize Workers"
        submit_jobs --> queue : "Add Jobs"
    }

    create_workers --> submit_jobs
```

```go
package main

import (
    "fmt"
    "time"
)

// (1)
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Println("worker", id, "started  job", j)
        time.Sleep(time.Second)
        fmt.Println("worker", id, "finished job", j)
        results <- j * 2
    }
}

func main() {

// setup channals for workers
    const numJobs = 5
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

// set 3 workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

// set number job then closed the jobs
    for j := 1; j <= numJobs; j++ {
        jobs <- j
    }
    close(jobs)

// wait for the job results
    for a := 1; a <= numJobs; a++ {
        <-results
    }
}
```

```terminal
worker 1 started  job 1
worker 2 started  job 2
worker 3 started  job 3
worker 1 finished job 1
worker 1 started  job 4
worker 2 finished job 2
worker 2 started  job 5
worker 3 finished job 3
worker 1 finished job 4
worker 2 finished job 5
real    0m2.358s // time usage
```

- Note on (1). This function would handle incoming job to worker channel then received to the result channel

## WaitGroup
#WaitGroup 
To wait for multiple goroutines to finish, we can use a waitGroup.

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
	"time"
)

func main() {
	var counter int32
	var wg sync.WaitGroup

	// Add 2 to the WaitGroup counter because we are creating two goroutines
	wg.Add(2)

	// Create a goroutine to increment the counter.
	go func() {
		for i := 0; i < 5; i++ {
			atomic.AddInt32(&counter, 1)
			fmt.Printf("Incremented: %d\n", atomic.LoadInt32(&counter))
			time.Sleep(time.Millisecond)
		}
		// Decrement the WaitGroup counter when the goroutine finishes
		wg.Done()
	}()

	// Create a goroutine to decrement the counter.
	go func() {
		for i := 0; i < 5; i++ {
			atomic.AddInt32(&counter, -1)
			fmt.Printf("Decremented: %d\n", atomic.LoadInt32(&counter))
			time.Sleep(time.Millisecond)
		}
		// Decrement the WaitGroup counter when the goroutine finishes
		wg.Done()
	}()

	// Wait for the goroutines to finish.
	wg.Wait()
	
// using `Load` it’s safe to atomically read a value even while other goroutines are (atomically) updating it.
	fmt.Printf("Final Value: %d\n", atomic.LoadInt32(&counter))
}

```

```terminal
Worker 5 starting
Worker 3 starting
Worker 4 starting
Worker 1 starting
Worker 2 starting
Worker 4 done
Worker 1 done
Worker 2 done
Worker 5 done
Worker 3 done
```

<mark style="background: #FFB86CA6;">The order of workers starting up and finishing is likely to be different for each invocation</mark>

- on (1). If a WaitGroup is explicitly **passed into functions,** **it should be done _by pointer_**.
- on (2). Wrap the worker call in a closure that makes sure to tell the WaitGroup that this worker is done. This way the worker itself does not have to be aware of the concurrency primitives involved in its execution.

Note that this approach has no straightforward way to propagate errors from workers. For more advanced use cases, consider using the [errgroup package](https://pkg.go.dev/golang.org/x/sync/errgroup).

