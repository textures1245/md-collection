[[Go Lectures]] #Go-Basic #Go-Concept 

Go supports _[pointers](https://en.wikipedia.org/wiki/Pointer_(computer_programming))_, allowing you to pass references to values and records within your program.

```merm
%%{init: { 'themeVariables': { 'fontSize': '10px', 'fontFamily': 'Inter'}},
{'flowchart':{'nodeSpacing': 40, 'rankSpacing': 20}}
}%%
graph TB
    A[Start]
    B[Declare a variable]
    C[Declare a pointer to the variable]
    E[Access the value using *pointer]
    F[Want to change the value?]
    X[Want to binding the value's mem pointer?]
    H[End]
    A --> B
    B --> C
    C --> E
    E --> |var pointer *original| F
    
    F --> |Change the value using *pointer =  newValue| X
    X --> |"funcA(&variable)"| H
```

- Pointer
	- **Pointer Receiver (\*)** :  this would be assigned for **Received** **variable's memory** for directly **handling original variable** (Mutable)
	- **Pointer Sender (\&)** :  Use for send\point variable's memory through another variable which can be called as **Pointer** 

```go
package main

import "fmt"

// doesn't work with pointers
func zeroval(ival int) {
    ival = 0
}

// work with pointers by taking params as pointer reciver (*)
func zeroptr(iptr *int) {
    *iptr = 0
}

func main() {
    i := 1
    fmt.Println("initial:", i)

    zeroval(i)
    fmt.Println("zeroval:", i)

    zeroptr(&i)
    fmt.Println("zeroptr:", i)

    fmt.Println("pointer:", &i)
}
```
