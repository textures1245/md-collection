[[Go Lectures]] #Go-Core #Go-Concept #Go-Package #Go-API

**Contents** #Go-HTTP-Contents
#HTTP-Client
#HTTP-Server
#Go-Context

Go standard library comes with excellent support for HTTP clients and servers in the `net/http` package. In this example we’ll use it to issue simple HTTP requests.

```go
package main

import (
    "bufio"
    "fmt"
    "net/http"
)

func main() {

    resp, err := http.Get("https://gobyexample.com")
    if err != nil {
        panic(err)
    }
    // ensures that the response body will be closed automatically
    defer resp.Body.Close()

    fmt.Println("Response status:", resp.Status)

// Print the first 5 lines of the response body.
    scanner := bufio.NewScanner(resp.Body)
    for i := 0; scanner.Scan() && i < 5; i++ {
        fmt.Println(scanner.Text())
    }

    if err := scanner.Err(); err != nil {
        panic(err)
    }
}
```

```terminal
Response status: 200 OK
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Go by Example</title>
```

## HTTP Server
#HTTP-Server

- A fundamental concept in `net/http` servers is _handlers_. A handler is an object implementing the `http.Handler` interface. A common way to write a handler is by using the `http.HandlerFunc` adapter on functions with the appropriate signature.

```go
package main

import (
    "fmt"
    "net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {
	// responed with "hello"
    fmt.Fprintf(w, "hello\n")
}


// reading all the HTTP request headers and echoing them into the response body.
func headers(w http.ResponseWriter, req *http.Request) {

    for name, headers := range req.Header {
        for _, h := range headers {
            fmt.Fprintf(w, "%v: %v\n", name, h)
        }
    }
}

func main() {

    http.HandleFunc("/hello", hello)
    http.HandleFunc("/headers", headers)

// Finally, we call the `ListenAndServe` with the port and a handler. `nil` tells it to use the default router we’ve just set up.
    http.ListenAndServe(":8090", nil)

}
```

```terminal
phakh$ go run . & curl localhost:8090/hello
[3] 82087
hello
```

## Context
#Go-Context 

**[Context](https://medium.com/@jamal.kaksouri/the-complete-guide-to-context-in-golang-efficient-concurrency-management-43d722f6eaea)** provides a mechanism to control the lifecycle, [[Concurrently]] operations, cancellation, and propagation of requests across multiple [[Goroutine]].

HTTP servers are useful for demonstrating the usage of context.Context for controlling cancellation. A Context carries deadlines, cancellation signals, and other request-scoped values across API boundaries and goroutines.

```merm
stateDiagram-v2
    state "Context Creation" as creation {
        state "Background Context" as background
        state "WithValue" as with_value
        state "WithCancel" as with_cancel
        state "WithTimeout" as with_timeout
        state "WithDeadline" as with_deadline
    }

    background --> with_value
    with_value --> with_cancel
    with_cancel --> with_timeout
    with_timeout --> with_deadline

    state "Context Usage" as usage {
        state "Check Cancellation" as check_cancellation
        state "Check Timeout" as check_timeout
        state "Check Deadline" as check_deadline
        state "Get Value" as get_value
    }

    with_deadline --> check_cancellation
    check_cancellation --> check_timeout
    check_timeout --> check_deadline
    check_deadline --> get_value

    state "Context Cancellation" as cancellation {
        state "Cancel Context" as cancel
        cancel --> "Cancel Signal"
        "Cancel Signal" --> "Clean Up Resources"
    }

    with_cancel --> cancel

    note left of background
        The background context is the root
        of all derived contexts. It should
        be used for top-level operations.
    end note

    note right of with_value
        WithValue creates a new context
        and associates a key-value pair
        with it.
    end note

    note right of with_cancel
        WithCancel creates a new context
        that can be canceled by calling
        the returned CancelFunc.
    end note

    

    note right of with_deadline
        WithDeadline creates a new context
        with a deadline. It will automatically
        cancel the context after the deadline
        expires.
    end note

    note left of check_cancellation
        Check if the context has been
        canceled by calling the Context.Err()
        method or using the Context.Done()
        channel.
    end note

    note left of check_deadline
        Check if the context has reached
        its deadline by calling the
        Context.Err() method or using the
        Context.Done() channel.
    end note

    note left of get_value
        Get the value associated with the
        context by calling the Context.Value()
        method with the appropriate key.
    end note

    note right of cancel
        Cancel the context by calling the
        CancelFunc returned by WithCancel.
        This will propagate the cancellation
        signal to all derived contexts.
    end note

    note right of "Clean Up Resources"
        When a context is canceled, all
        operations associated with that
        context should clean up resources
        and exit gracefully.
    end note
```

```go
package main

import (
    "fmt"
    "net/http"
    "time"
)

func hello(w http.ResponseWriter, req *http.Request) {

    ctx := req.Context()
    fmt.Println("server: hello handler started")
    defer fmt.Println("server: hello handler ended")

    select {
    case <-time.After(10 * time.Second):
        fmt.Fprintf(w, "hello\n")
    case <-ctx.Done():

        err := ctx.Err()
        fmt.Println("server:", err)
        internalError := http.StatusInternalServerError
        http.Error(w, err.Error(), internalError)
    }
}

func main() {

    http.HandleFunc("/hello", hello)
    http.ListenAndServe(":8090", nil)
}
```

#### Example: Managing Concurrent API Requests

```go
package main

import (
	"context"
	"fmt"
	"net/http"
	"time"
)

func main() {
	// create context with timeout in 5 sec
	ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
	defer cancel()

	urls := []string{
		"https://api.example.com/users",
		"https://api.example.com/products",
		"https://api.example.com/orders",
	}

	results := make(chan string)

	for _, url := range urls {
		go fetchAPI(ctx, url, results)
	}

	// wait for responses
	for range urls {
		fmt.Println(<-results)
	}
}

func fetchAPI(ctx context.Context, url string, results chan<- string) {
	// create request
	req, err := http.NewRequestWithContext(ctx, "GET", url, nil)
	if err != nil {
		results <- fmt.Sprintf("Error creating request for %s: %s", url, err.Error())
		return
	}

	// set client then send req to client (used by Get, Head, and Post.)
	client := http.DefaultClient
	resp, err := client.Do(req)
	if err != nil {
		results <- fmt.Sprintf("Error making request to %s: %s", url, err.Error())
		return
	}
	defer resp.Body.Close()

	results <- fmt.Sprintf("Response from %s: %d", url, resp.StatusCode)
}
```

```terminal
Response from https://api.example.com/users: 200  
Response from https://api.example.com/products: 200  
Response from https://api.example.com/orders: 200
```

#### Example: Using Context in Database Operations
```go
package main

import (
 "context"
 "database/sql"
 "fmt"
 "time"

 _ "github.com/lib/pq"
)

func main() {
 ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
 defer cancel()

 db, err := sql.Open("postgres", "postgres://username:password@localhost/mydatabase?sslmode=disable")
 if err != nil {
  fmt.Println("Error connecting to the database:", err)
  return
 }
 defer db.Close()

 // Execute the database query with the custom context
 rows, err := db.QueryContext(ctx, "SELECT * FROM users")
 if err != nil {
  fmt.Println("Error executing query:", err)
  return
 }
 defer rows.Close()

 // Process query results
}
```

**Best Practice **
1. Pass Context Explicitly: Always pass the context as an explicit argument to functions or goroutines instead of using global variables. This makes it easier to manage the context’s lifecycle and prevents potential data races.
2. Use context.TODO(): If you are unsure which context to use in a particular scenario, consider using `context.TODO()`. However, make sure to replace it with the appropriate context later.
3. Avoid Using context.Background(): Instead of using `context.Background()` directly, create a specific context using `context.WithCancel()` or `context.WithTimeout()` to manage its lifecycle and avoid resource leaks.
4. Prefer Cancel Over Timeout: Use `context.WithCancel()` for cancellation when possible, as it allows you to explicitly trigger cancellation when needed. `context.WithTimeout()` is more suitable when you need an automatic cancellation mechanism.
5. Keep Context Size Small: Avoid storing large or unnecessary data in the context. Only include the data required for the specific operation.
6. Avoid Chaining Contexts: Chaining contexts can lead to confusion and make it challenging to manage the context hierarchy. Instead, propagate a single context throughout the application.
7. Be Mindful of Goroutine Leaks: Always ensure that goroutines associated with a context are properly closed or terminated to avoid goroutine leaks.