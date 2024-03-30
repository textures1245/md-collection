[[Go Lectures]] #Go-Core #Go-Concept #Go-Package 

Go standard library provides straightforward tools for outputting logs from Go programs, with the [log](https://pkg.go.dev/log) package for free-form output and the [log/slog](https://pkg.go.dev/log/slog) package for structured output.

```go
package main

import (
    "bytes"
    "fmt"
    "log"
    "os"

    "log/slog"
)

func main() {

    log.Println("standard logger")

    log.SetFlags(log.LstdFlags | log.Lmicroseconds)
    log.Println("with micro")

    log.SetFlags(log.LstdFlags | log.Lshortfile)
    log.Println("with file/line")

    mylog := log.New(os.Stdout, "my:", log.LstdFlags)
    mylog.Println("from mylog")

    mylog.SetPrefix("ohmy:")
    mylog.Println("from mylog")

    var buf bytes.Buffer
    buflog := log.New(&buf, "buf:", log.LstdFlags)

    buflog.Println("hello")

    fmt.Print("from buflog:", buf.String())

    jsonHandler := slog.NewJSONHandler(os.Stderr, nil)
    myslog := slog.New(jsonHandler)
    myslog.Info("hi there")

    myslog.Info("hello again", "key", "val", "age", 25)
}
```