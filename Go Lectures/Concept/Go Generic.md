[[Go Lectures]] #Go-Basic #Go-Concept 

	Starting with version 1.18, Go has added support for _generics_, also known as _type parameters

- **Syntax**
1. Generic Function (also known as **Type Parameter**)
```go
func name[T any](params T) T {}

func MapKeys[K comparable, V any](m map[K]V) []K {
    r := make([]K, 0, len(m))
    for k := range m {
        r = append(r, k)
    }
    return r
}
```
- As an example of a generic function, `MapKeys` takes a map of any type and returns a slice of its keys. This function has two type parameters - `K` and `V`; `K` has the `comparable` _constraint_, meaning that we can compare values of this type with the `==` and `!=` operators. This is required for map keys in Go. `V` has the `any` constraint, meaning that it’s not restricted in any way (`any` is an alias for `interface{}`).

2. Generic Method Function
```go
// create type strcutor
type List[T any] struct {
	head, tail *element[T]
}

type element[T any] struct {
	next *element[T]
	val  T
}

// create type method
func (lst *List[T]) Push(v T) {
	if lst.tail == nil {
		lst.head = &element[T]{val: v}
		lst.tail = lst.head
	} else {
		lst.tail.next = &element[T]{val: v}
		lst.tail = lst.tail.next
	}
}
```


 - Usage
```go
[FILE_NAME=findMax.go]

package generic

import (
	"fmt"
	"reflect"
	"unicode/utf16"
)

func Map[T, U any](ts []T, f func(v T) U) []U {
	arr := make([]U, len(ts))
	// Assign the result of applying the function to the current element to the corresponding index in the new array.
	for i := range ts {
		arr[i] = f(ts[i])
	}
	return arr
}

func MapToBytes[T any](ts []T) []byte {
	// Convert the value to a string, encode it as UTF-16, and return the first byte.
	return Map(ts, func(v T) byte {
		return byte(utf16.Encode([]rune(fmt.Sprintf("%v", v)))[0])
	})
}

func FindMaxVal[T any](arr []T) (T, string) {
	// Return the zero value of the slice's type and an empty string.
	if len(arr) == 0 {
		return reflect.Zero(reflect.TypeOf(arr[0])).Interface().(T), ""
	}

	byteArr := MapToBytes(arr)

	max, index := byteArr[0], 0
	for i, v := range byteArr {
		if v > max { // Check if the current value is greater than the current maximum.
			max, index = v, i // Update the maximum value and its index.
		}
	}

	return arr[index], reflect.TypeOf(arr[index]).String()
}
```

- Test Result
```go
func main() {
	var intArr = []int16{1, 2, 3, 4}
	var intStr = []string{"a", "b", "c", "d"}

	maxInt, t1 := generic.FindMaxVal(intArr)
	maxStr, t2 := generic.FindMaxVal(intStr)

	fmt.Println("maxInt:", maxInt, "type:", t1)
	fmt.Println("maxStr:", maxStr, "type:", t2)
}


```

```compile
maxInt: 4 type: int16
maxStr: d type: string
```