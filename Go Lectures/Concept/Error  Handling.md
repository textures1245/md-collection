[[Go Lectures]] #Go-Concept #Go-Basic 

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

- **Custom Error**
	- Create custom Error with type structor and for custom handling and usage repeating.  
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
	
	In this example. We could see the that we had 
	- `argError` struct and `Error()` method that return `error format`from struct pointer receiver 
	- f() function for error handler
	- and the main function that would check if err is of type argError or not by using `error.As`
	  
	`errors.Is` checks that a given error (or any error in its chain) matches a specific error value. This is especially useful with wrapped or nested errors, allowing you to identify specific error types or sentinel errors in a chain of errors.