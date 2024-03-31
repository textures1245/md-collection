[[Go Practical Examples]] #Go-Examples

Knowledge requirements
[[Go Generic]]
[[Timeout]]

Src: [main](https://github.com/textures1245/go-practical-examples/blob/main/main.go), [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/generic/index.go) 

**Assignment** 
- Implement a generic function and structs and achieved assign by the following 
	- given the generic function that takes a **slice** **of any type** and returns the **sum or concatenation** of its elements.
	- each time that had sum the elements, **keep track to last slice of its pointer elements.** And it can **print** **slice of elements** (convert pointers to original elements).
```go
package generic

import "reflect"

//- Write a program that simulates a client-server interaction, where the client sends a request and waits for a response with a timeout.

// -\ Implement a generic function and structs and achieved assign by the following
//? each time that had sum the elements, **keep track to last slice of its pointer elements.** And it can **print** **slice of elements** (convert pointers to original elements).

// using LinkList algo to keep track of the last slice of its pointer elements
type List[T any] struct {
	head, tail *element[T]
}

type element[T any] struct {
	next *element[T]
	val  T
}

// pushing the elements to the list while element.next is pointer and element.val is the value
func (lst *List[T]) Push(v T) {
	if lst.tail == nil {
		lst.head = &element[T]{val: v}
		lst.tail = lst.head
	} else {
		lst.tail.next = &element[T]{val: v}
		lst.tail = lst.tail.next
	}
}

func (lst *List[T]) GetElemsSumHistory() []T {
	var elems []T
	for e := lst.head; e != nil; e = e.next {
		elems = append(elems, e.val)
	}
	return elems
}

//? given the generic function that takes a **slice** **of any type** and returns the **sum or concatenation** of its elements.

type Generic[T any] struct {
	// create a slice that will store LinkList of elements
	Pos List[T]
}

// return the interface since the type of the elements is unknown
// Note: we can't return value as generic type since generic type can't be operated

func (g *Generic[T]) Sum(arr []T) interface{} {
	if len(arr) == 0 {
		return nil
	}

	g.Pos = List[T]{}
	for _, v := range arr {
		g.Pos.Push(v)
	}

	// check the type of the elements in the slice
	switch reflect.TypeOf(arr[0]).Kind() {
	case reflect.Int:
		sum := 0
		for _, v := range arr {
			sum += reflect.ValueOf(v).Interface().(int)
		}
		return sum
	case reflect.Float64, reflect.Float32:
		sum := 0.0
		for _, v := range arr {
			sum += reflect.ValueOf(v).Interface().(float64)
		}
		return sum

	case reflect.String:
		concatenation := ""
		for _, v := range arr {
			concatenation += reflect.ValueOf(v).Interface().(string)
		}
		return concatenation
	default:
		return nil
	}
}
```

```go
func genericTask() {
	// -\ Implement a generic function and structs and achieved assign by the following
	sum1 := generic.Generic[int]{}
	fmt.Println(sum1.Sum([]int{1, 2, 3, 4, 5}))
	fmt.Println(sum1.Pos.GetElemsSumHistory())

	sum2 := generic.Generic[string]{}
	fmt.Println(sum2.Sum([]string{"a", "b", "c", "d", "e"}).(string))
	fmt.Println(sum2.Pos.GetElemsSumHistory())
	fmt.Println(sum2.Sum([]string{"a", "b", "c", "d", "e", "f"}).(string))
	fmt.Println(sum2.Pos.GetElemsSumHistory())
}
```

```terminal
15
[1 2 3 4 5]
abcde
[a b c d e]
abcdef
[a b c d e f]
```

- Write a program that simulates a client-server interaction, where the client sends a request and waits for a response with a timeout.

```go
type Client struct {
	Results chan string
}

type Server struct {
}

func (c *Client) OnSendReqs(urls []string) {
	// set request timeout to 3 seconds
	const timeout = 3 * time.Second
	ctx, cancel := context.WithTimeout(context.Background(), timeout)
	defer cancel()

	// showcase for deadline exceed, we wait req to timeout
	time.Sleep(1000 + timeout)

	s := Server{}

	for _, url := range urls {
		go s.HandleReq(ctx, url, c.Results)
	}

	for range urls {
		fmt.Println(<-c.Results)
	}
}

func (s *Server) HandleReq(ctx context.Context, url string, results chan<- string) {
	req, err := http.NewRequestWithContext(ctx, "GET", url, nil)

	if err != nil {
		panic(err)
	}

	client := http.DefaultClient // set client for create http response back
	resp, err := client.Do(req)
	if err != nil {
		results <- fmt.Sprintf("Error making request to %s: %s", url, err.Error())
		return
	}
	defer resp.Body.Close()

	results <- fmt.Sprintf("Response from %s: %d", url, resp.StatusCode)
}
```

```go
func genericTask() {
	//- Write a program that simulates a client-server interaction, where the client sends a multiply requests and waits for a response with a timeout.
	urls := []string{"https://json3placeholder.typicode.com/todos/1", "https://jsonplac3eholder.typicode.com/todos/2", "https://jsonplac3eholder.typicode.com/todos/3"}
	task3 := generic.Client{Results: make(chan string, len(urls))}

	task3.OnSendReqs(urls)
}
```

```terminal
// case that request had succeed
Response from https://jsonplaceholder.typicode.com/todos/1: 200
Response from https://jsonplaceholder.typicode.com/todos/3: 200
Response from https://jsonplaceholder.typicode.com/todos/2: 200

// case deadline exceed, we wait req to timeout
Error making request to https://json3placeholder.typicode.com/todos/1: Get "https://json3placeholder.typicode.com/todos/1": context deadline exceeded
Error making request to https://jsonplac3eholder.typicode.com/todos/2: Get "https://jsonplac3eholder.typicode.com/todos/2": context deadline exceeded
Error making request to https://jsonplac3eholder.typicode.com/todos/3: Get "https://jsonplac3eholder.typicode.com/todos/3": context deadline exceeded
```
