[[Go Lectures]] #Go-Basic #Go-Concept 

**Content**
#Closure
#defer
#Go-ENV

## Closure
#Closure
Go supports [_anonymous functions_](https://en.wikipedia.org/wiki/Anonymous_function), which can form [_closures_](https://en.wikipedia.org/wiki/Closure_(computer_science)). Anonymous functions are useful when you want to define a function inline without having to name it.

```go
package main

import "fmt"

func intSeq() func() int {
    i := 0
    // closure function
    return func() int {
        i++
        return i
    }
}

func main() {

    nextInt := intSeq()

    fmt.Println(nextInt())
    fmt.Println(nextInt())
    fmt.Println(nextInt())

    newInts := intSeq()
    fmt.Println(newInts())
}
```

## Defer
#defer

*Defer* is used to ensure that a function call is performed later in a program’s execution, usually for purposes of **cleanup** or prevent **deadlock.** if you working with [[Concurrently]]. defer is **often used where e.g. ensure and finally would be used in other languages.**

```go
func main() {
	
    f := createFile("/tmp/defer.txt")
    defer closeFile(f) // Will run after `writeFile` finished
    writeFile(f)
}
```

- Immediately after getting a file object with `createFile`, we defer the closing of that file with `closeFile`. This will be executed at the end of the enclosing function (`main`), after `writeFile` has finished.

```go
func (c *Container) inc(name string) {

    c.mu.Lock()
    defer c.mu.Unlock() // should do
    c.counters[name]++
    c.mu.Unlock() // don't do
}
```

- The `Unlock()` method is called directly after incrementing the counter. If an error occurs between the `Lock()` and `Unlock()` calls (for example, if incrementing the counter panics), using `Unlock()` alone may not be called, which would result in a deadlock.

## Go Environment Variables
#Go-ENV 

```go
package main

import (
    "fmt"
    "os"
    "strings"
)

func main() {

// setter
    os.Setenv("FOO", "1")
// getting
    fmt.Println("FOO:", os.Getenv("FOO"))
    fmt.Println("BAR:", os.Getenv("BAR"))

    fmt.Println()
    for _, e := range os.Environ() {
        pair := strings.SplitN(e, "=", 2)
        fmt.Println(pair[0])
    }
}
```

```
FOO: 1
BAR: 

TERM_PROGRAM
PATH
SHELL
...
FOO
```

- If we set `BAR` in the environment first, the running program picks that value up.

```terminal
$ BAR=2 go run environment-variables.go
FOO: 1
BAR: 2
```

