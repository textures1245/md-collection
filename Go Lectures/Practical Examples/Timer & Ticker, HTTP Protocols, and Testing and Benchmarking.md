[[Go Practical Examples]] #Go-Examples 

Knowledge Requirement
	[[Timer & Ticker]]
	[[HTTP Protocols]]
	[[Testing and Bechmarking]]

Src: [main](https://github.com/textures1245/go-practical-examples/blob/main/main.go)

Assignments 
- Write a program that uses a timer to periodically then run callback function to manipulated data
- src: [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/timer/index.go)
```go
package timer

import (
	"fmt"
	"time"
)

type OnTick[T any] struct{}

// create ticker handler, every tick will call the func from arg then return the result to the channel
func (o *OnTick[T]) TimestampTicker(t *time.Ticker, done <-chan bool, res chan<- T, f func() T) {

	for {
		select {
		case <-done:
			t.Stop()
			// closed the channel for prevent any incoming requests
			defer close(res)
			return
		case t := <-t.C:
			go func() {
				fmt.Println("Tick at", t)
				res <- f()
			}()

		}
	}
}

func (o *OnTick[T]) StopTicker(done chan<- bool) {
	done <- true
	fmt.Println("Ticker stopped")
}
```

```go
[MAIN]
t := timer.OnTick[int]{}
	ticker := time.NewTicker(500 * time.Millisecond)
	num := 0
	done := make(chan bool, 1)
	results := make(chan int)

	incre := func(n *int) {
		*n++
	}

	go t.TimestampTicker(ticker, done, results, func() int {
		incre(&num)
		return num
	})

	time.Sleep(4 * time.Second)
	t.StopTicker(done)
	// since we closed the channel, now we can safely range elems over the channel
	for n := range results {
		fmt.Println(n)

	}
```

```bash
Tick at 2024-04-02 15:12:35.200787 +0700 +07 m=+0.501470085
Tick at 2024-04-02 15:12:35.700778 +0700 +07 m=+1.001469460
Tick at 2024-04-02 15:12:36.200812 +0700 +07 m=+1.501510376
Tick at 2024-04-02 15:12:36.700785 +0700 +07 m=+2.001491335
Tick at 2024-04-02 15:12:37.200778 +0700 +07 m=+2.501492001
Tick at 2024-04-02 15:12:37.70077 +0700 +07 m=+3.001490918
Tick at 2024-04-02 15:12:38.200747 +0700 +07 m=+3.501475418
Ticker stopped
1
2
3
4
5
6
```

- Implement a simple HTTP server that serves static files and handles different HTTP methods (GET, POST, etc.)
- src: [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/https/index.go)
```go
type Res struct {
	Status  int
	context string
}

type Req struct {
	Target string
	Verb   string
	Dat    url.Values
}

type Server struct{}

// - Implement a simple HTTP server that serves static files and handles different HTTP methods (GET, POST, etc.).
func (s *Server) HandleReq(reqDat Req, res chan Res) {

	const timeout = 3 * time.Second
	ctx, cancel := context.WithTimeout(context.Background(), timeout)

	// showcase for deadline exceed, we wait req to timeout
	// time.Sleep(1000 + timeout)

	defer cancel()

	switch strings.ToUpper(reqDat.Verb) {
	case "GET":
		HttpHandler(ctx, reqDat, res)
	case "POST":
		HttpHandler(ctx, reqDat, res)
	default:
		panic("Method not allowed")
	}

}

func HttpHandler(ctx context.Context, reqDat Req, results chan<- Res) {

	var fc io.Reader

	if reqDat.Dat != nil {
		fc = strings.NewReader(reqDat.Dat.Encode())
	}

	req, err := http.NewRequestWithContext(ctx, strings.ToUpper(reqDat.Verb), reqDat.Target, fc)
	if err != nil {
		panic(err)
	}

	if reqDat.Dat != nil {
		req.Header.Add("Content-Type", "application/x-www-form-urlencoded")
	}

	client := http.DefaultClient // set client for create http response back
	resp, err := client.Do(req)
	if err != nil {
		results <- Res{context: fmt.Sprintf("Error making request to %s: %s", reqDat.Target, err.Error()), Status: 500}
		return
	}
	defer resp.Body.Close()

	results <- Res{context: fmt.Sprintf("Response from %s: %d", reqDat.Target, resp.StatusCode), Status: resp.StatusCode}
}
```

```go
[MAIN]
s := https.Server{}

reqs := []https.Req{
	{Target: "https://jsonplaceholder.typicode.com/todos/1", Verb: "GET", Dat: nil},
	{Target: "https://jsonplaceholder.typicode.com/todos/2", Verb: "GET", Dat: nil},
}
var wg sync.WaitGroup

res := make(chan https.Res, len(reqs))

for _, req := range reqs {
	wg.Add(1)
	go func(req https.Req, wg *sync.WaitGroup) {
		defer wg.Done()
		s.HandleReq(req, res)
	}(req, &wg)
}
wg.Wait()

close(res) // close channel for no incoming reqs

for r := range res {
	fmt.Println(r)
}
```

```bash
{200 Response from https://jsonplaceholder.typicode.com/todos/2: 200}
{200 Response from https://jsonplaceholder.typicode.com/todos/1: 200}
```

- Create a test suite for a function or package you have written, and include benchmarks to measure its performance.
- src: [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/https/http_test.go)
```go
package https

import (
	"context"
	"fmt"
	"testing"
	"time"
)

// Unit test for HttpHandler
func exceptedResStatusToBe(reqDat Req, excpStatus int, results chan Res, t *testing.T) Res {
	go HttpHandler(context.Background(), reqDat, results)
	res := <-results
	if res.Status != excpStatus {
		t.Errorf("Expected status to be %d, but got %d", excpStatus, res.Status)
	}
	return res

}

func TestHttpHandler(t *testing.T) {
	// Test code goes here

	results := make(chan Res)

// using `Table-Driven Test` approved 
	test := []struct {
		reqDat Req
		wants  int
	}{
		{Req{Target: "https://jsonplaceholder.typicode.com/todos/1", Verb: "GET", Dat: nil}, 200},
		{Req{Target: "https://jsonplaceholder.typicode.com/todos/2", Verb: "GET", Dat: nil}, 500},
		{Req{Target: "https://jsonplaceholder.typicode.com/todos/3", Verb: "GET", Dat: nil}, 500},
		{Req{Target: "https://jsonplaceholder.typicode.com/todos/4", Verb: "GET", Dat: nil}, 200},
		{Req{Target: "https://jsonplaceholder.typicode.com/todos/5", Verb: "GET", Dat: nil}, 200},
	}

	for _, tt := range test {
		testname := fmt.Sprintf("%s,%s", tt.reqDat.Target, tt.reqDat.Verb)
		t.Run(testname, func(t *testing.T) {
			exceptedResStatusToBe(tt.reqDat, tt.wants, results, t)
		})
	}
}

// benchmarking for HttpHandler
func BenchmarkTestHttpHandlerWithRateLimiter(b *testing.B) {
	limiter := time.Tick(100 * time.Millisecond) // set limiter = 200 mills

	results := make(chan Res)
	reqDat := Req{Target: "https://jsonplaceholder.typicode.com/todos/1", Verb: "GET", Dat: nil}
	for i := 0; i < b.N; i++ {
		<-limiter
		go HttpHandler(context.Background(), reqDat, results)
	}
}

func BenchmarkTestHttpHandlerWithBurstyLimiter(b *testing.B) {
// allow short bursts of requests in our rate limiting for performance increment 
	buffers := 20
	burstyLimiter := make(chan time.Time, buffers)

	// Fill up the channel to represent allowed bursting.
	for i := 0; i < buffers; i++ {
		burstyLimiter <- time.Now()
	}

	// using goroutine to tick every 200 mills to fill up the channal if it can.
	go func() {
		for t := range time.Tick(100 * time.Millisecond) {
			burstyLimiter <- t
		}
	}()

	results := make(chan Res)
	reqDat := Req{Target: "https://jsonplaceholder.typicode.com/todos/1", Verb: "GET", Dat: nil}

	for req := 0; req < b.N; req++ {
		<-burstyLimiter // limit reqs followed bursty limiter
		go HttpHandler(context.Background(), reqDat, results)
	}
}
```

- Testing on `TestHttpHandler` 

```bash
Traiphakhs-MacBook-Air:https phakh$ go test -v
=== RUN   TestHttpHandler
=== RUN   TestHttpHandler/https://jsonplaceholder.typicode.com/todos/1,GET
=== RUN   TestHttpHandler/https://jsonplaceholder.typicode.com/todos/2,GET
    http_test.go:16: Expected status to be 500, but got 200
=== RUN   TestHttpHandler/https://jsonplaceholder.typicode.com/todos/3,GET
    http_test.go:16: Expected status to be 500, but got 200
=== RUN   TestHttpHandler/https://jsonplaceholder.typicode.com/todos/4,GET
=== RUN   TestHttpHandler/https://jsonplaceholder.typicode.com/todos/5,GET
--- FAIL: TestHttpHandler (0.38s)
    --- PASS: TestHttpHandler/https://jsonplaceholder.typicode.com/todos/1,GET (0.17s)
    --- FAIL: TestHttpHandler/https://jsonplaceholder.typicode.com/todos/2,GET (0.05s)
    --- FAIL: TestHttpHandler/https://jsonplaceholder.typicode.com/todos/3,GET (0.05s)
    --- PASS: TestHttpHandler/https://jsonplaceholder.typicode.com/todos/4,GET (0.05s)
    --- PASS: TestHttpHandler/https://jsonplaceholder.typicode.com/todos/5,GET (0.05s)
FAIL
exit status 1
FAIL    github.com/textures1245/practical-examples/examples/https       0.725s
```

- Benchmark testing on `BenchmarkTestHttpHandlerWithBurstyLimiter` and `BenchmarkTestHttpHandlerWithRateLimiter` (all the unit tests should all be PASSED before running the benchmarking)

```bash
Traiphakhs-MacBook-Air:https phakh$ go test -bench=.
goos: darwin
goarch: arm64
pkg: github.com/textures1245/practical-examples/examples/https
BenchmarkTestHttpHandlerWithRateLimiter-8             10         100012533 ns/op
BenchmarkTestHttpHandlerWithBurstyLimiter-8          100          80000720 ns/op
PASS
ok      github.com/textures1245/practical-examples/examples/https       9.952s
```