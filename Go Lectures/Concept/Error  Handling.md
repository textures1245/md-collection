[[Go Lectures]] #Go-Concept #Go-Basic 

**Content**
#Error-Interface 
#Custom-Error
#Panic
#Recovering



communicate errors via an explicit, separate return value. This contrasts with the exceptions used in languages like Java and Ruby and the overloaded single result


```merm
flowchart LR
  Start((Start)) -->|Create Base Error Interface| A[errors.New<>]
  A --> |Imp with function handler| B[func handler<> error]
  B --> |nil| C[return value]
  B --> |error format| C
  C --> D{is nil?}
  C --> D{is nil?}
  D --> |true| E[show error format]
  E --> F[end]
  D --> |false| F[end]
  

```
## Error Interface
#Error-Interface 
- **Error Interface** 
	- By convention, errors are the last return value and have type `error`, a built-in interface.
	- `errors.New` constructs a basic `error` value with the given error message
	- A `nil` value in the error position indicates that there was no error.
```go
func f(arg int) (int, error) {
    if arg == 42 {

        return -1, errors.New("can't work with 42")
    }

    return arg + 3, nil
}
```


- **Error Format**
```go
var ErrOutOfTea = fmt.Errorf("no more tea available")
var ErrPower = fmt.Errorf("can't boil water")

func makeTea(arg int) error {
    if arg == 2 {
        return ErrOutOfTea
    } else if arg == 4 {

        return fmt.Errorf("making tea: %w", ErrPower)
    }
    return nil
}
```

- Usage
```go
func main() {
    for _, i := range []int{7, 42} {

        if r, e := f(i); e != nil {
            fmt.Println("f failed:", e)
        } else {
            fmt.Println("f worked:", r)
        }
    }

    for i := range 5 {
        if err := makeTea(i); err != nil {

            if errors.Is(err, ErrOutOfTea) {
                fmt.Println("We should buy new tea!")
            } else if errors.Is(err, ErrPower) {
                fmt.Println("Now it is dark.")
            } else {
                fmt.Printf("unknown error: %s\n", err)
            }
            continue
        }

        fmt.Println("Tea is ready!")
    }
}
```


## **Custom Error**
#Custom-Error 

Create custom Error with type structor and for custom handling and usage repeating.  
```go
package main

import (
	"errors"
	"fmt"
)

type argError struct {
	arg     int
	message string
}

// implement Error() method for argError with struct pointer receiver
func (e *argError) Error() string {
	return fmt.Sprintf("%d - %s", e.arg, e.message)
}

func f(arg int) (int, error) {
	if arg == 42 {

		// return -1 and pointer sender to argError
		return -1, &argError{arg, "can't work with it"}
	}
	return arg + 3, nil
}

func main() {

	_, err := f(42)
	// create pointer to argError
	var ae *argError
	// check if err is of type argError
	if errors.As(err, &ae) {
		fmt.Println(ae.arg)
		fmt.Println(ae.message)
		fmt.Println(ae.Error())
	} else {
		fmt.Println("err doesn't match argError")
	}
}

```
	
- `argError` struct and `Error()` method that return `error format`from struct pointer receiver 
	- f() function for error handler
	- and the main function that would check if err is of type argError or not by using `error.As`
- `errors.Is` checks that a given error (or any error in its chain) matches a specific error value. 
	- This is especially useful with wrapped or nested errors, allowing you to identify specific error types or sentinel errors in a chain of errors.

## Panic 
#Panic 

```merm
flowchart TD
    start([Start]) --> normal_flow{Normal Flow}

    normal_flow --No Errors--> continue_execution

    normal_flow --Error Occurs--> handle_error{Possible to Handle Error?}

    handle_error --Yes--> handle_gracefully[Handle Error Gracefully]
    handle_gracefully --> continue_execution

    handle_error --No--> panic_condition{Panic Condition Met?}

    panic_condition --Yes--> trigger_panic[Trigger panic]
    trigger_panic --> deferred_calls[Execute Deferred Calls]
    deferred_calls --> stack_unwind[Unwind Call Stack]
    stack_unwind --> recover_block{Recover Block Found?}

    recover_block --Yes--> handle_panic[Handle Panic]
    handle_panic --> continue_execution

    recover_block --No--> terminate_program[Terminate Program]

    panic_condition --No--> terminate_program

    continue_execution --> normal_flow

    terminate_program([End])

    style start fill:#0db9d7,stroke:#333,stroke-width:2px
    style normal_flow fill:#f9f,stroke:#333,stroke-width:4px
    style continue_execution fill:#b8e186,stroke:#333,stroke-width:2px
    style handle_error fill:#ffd31d,stroke:#333,stroke-width:2px
    style handle_gracefully fill:#b8e186,stroke:#333,stroke-width:2px
    style panic_condition fill:#ffd31d,stroke:#333,stroke-width:2px
    style trigger_panic fill:#e85d75,stroke:#333,stroke-width:2px
    style deferred_calls fill:#b8e186,stroke:#333,stroke-width:2px
    style stack_unwind fill:#b8e186,stroke:#333,stroke-width:2px
    style recover_block fill:#ffd31d,stroke:#333,stroke-width:2px
    style handle_panic fill:#b8e186,stroke:#333,stroke-width:2px
    style terminate_program fill:#e85d75,stroke:#333,stroke-width:2px

```

A **panic** typically means something **went unexpectedly wrong**. Mostly **we use it to fail fast** on errors that shouldn’t occur during normal operation, or that we aren’t prepared to handle gracefully.
- Note: panic is used to handle unrecoverable errors in Go. It should be used judiciously and only in situations where it's impossible to continue execution safely.

```go
package main

import "os"

func main() {

// When first panic in `main` fires, the program exits without reaching the rest of the code
    panic("a problem")

    _, err := os.Create("/tmp/file")
    if err != nil {
        panic(err)
    }
}
```

- A common use of panic is to abort if a function returns an error value that we don’t know how to (or want to) handle. Here’s an example of `panic`king if we get an unexpected error when creating a new file.

```bash
panic: a problem
goroutine 1 [running]:
main.main()
    /.../panic.go:12 +0x47
...
exit status 2
```

## Recovering
#Recovering 

A `recover` can stop a `panic` from aborting the program and let it continue with execution instead.

- An example of where this can be useful: a server wouldn’t want to crash if one of the client connections exhibits a critical error. Instead, the server would want to close that connection and continue serving other clients

```merm
sequenceDiagram
    participant Server
    actor Client1
    actor Client2
    actor Client3

    Client1->>Server: Request
    Server->>Client1: Response
    Note right of Server: Handling Client1 request

    Client2->>Server: Request
    Note right of Server: Handling Client2 request

    Client3->>Server: Request
    Note right of Server: Handling Client3 request

    Client2-->>Server: Critical Error
    Note right of Server: Critical error from Client2

    Server->>Server: Handle Critical Error
    Server->>Server: (1) Recovering from the panic

    Server->>Client2: Close Connection
    Note right of Server: Recover from panic and close Client2 connection

    Server->>Client1: Continue Serving
    Note right of Server: Continue serving Client1

    Server->>Client3: Continue Serving
    Note right of Server: Continue serving Client3
```

```go
package main

import "fmt"

func mayPanic() {
	panic("a problem")
}

func main() {

// (1) recover must be called within a deferred function. to be trigger when encrosing function panic (panic triggered)
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("Recovered. Error:\n", r)
			closeConnection(Client2) // closed connection to this client, then continue serving others.
		}
	}()

	mayPanic()

// this won't run since the function got panicked before it executed
	fmt.Println("After mayPanic()")
}
```

```bash
Recovered. Error:
 a problem
```