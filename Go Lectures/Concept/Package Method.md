[[Go Lectures]] #Go-Core #Go-Concept #Go-Basic 

**Content**
#Sorting
#Sorting-By-Function
#String-Functions
#String-Formatting
#Go-JSON
#Time
#Time-Formatting-via-Parsing 

## Sorting
#Sorting
Go’s slices package implements sorting for builtins and user-defined types. We’ll look at sorting for builtins first.
	Sorting functions are generic, and work for any _ordered_ built-in type. For a list of ordered types, see [cmp.Ordered](https://pkg.go.dev/cmp#Ordered).

```go
package main

import (
    "fmt"
    "slices"
)

func main() {

    strs := []string{"c", "a", "b"}
    slices.Sort(strs)
    fmt.Println("Strings:", strs)

    ints := []int{7, 2, 4}
    slices.Sort(ints)
    fmt.Println("Ints:   ", ints)

    s := slices.IsSorted(ints)
    fmt.Println("Sorted: ", s)
}
```

## Sorting Function
#Sorting-By-Function 

When comes to Custom Sorting. We can do this in Go. For example, suppose we wanted to sort strings by their length instead of alphabetically.

```go
package main

import (
    "cmp"
    "fmt"
    "slices"
)

func main() {
    fruits := []string{"peach", "banana", "kiwi"}

// go provided cmp.Compare package for value comparison
    lenCmp := func(a, b string) int {
        return cmp.Compare(len(a), len(b))
    }

// used SortFunc* with cmp package for custom sorting
    slices.SortFunc(fruits, lenCmp)
    fmt.Println(fruits)

    type Person struct {
        name string
        age  int
    }

    people := []Person{
        Person{name: "Jax", age: 37},
        Person{name: "TJ", age: 25},
        Person{name: "Alex", age: 72},
    }

// (1)
    slices.SortFunc(people,
        func(a, b Person) int {
            return cmp.Compare(a.age, b.age)
        })
    fmt.Println(people)
}
```

```bash
[kiwi peach banana]
[{TJ 25} {Jax 37} {Alex 72}]
```

- Note on (1): if the `Person` struct is large, you may want the slice to contain `*Person` instead and adjust the sorting function accordingly. If in doubt, [benchmark](https://gobyexample.com/testing-and-benchmarking)!

```go
// changed to Pointer Array instead for the performance
	people := []*Person{
		{name: "Jax", age: 37},
		{name: "TJ", age: 25},
		{name: "Alex", age: 72},
	}

// since `SortFunc` doesn't accept pointer. we can using `sort` go standard lib for handle custom sorting
	sort.Slice(people, func(i, j int) bool {
		return people[i].age < people[j].age
	})

	for _, person := range people {
		fmt.Println(person.name, person.age)
	}
```

## String Functions
#String-Functions 

```go
package main

import (
    "fmt"
    s "strings"
)

var p = fmt.Println

func main() {

    p("Contains:  ", s.Contains("test", "es"))
    p("Count:     ", s.Count("test", "t"))
    p("HasPrefix: ", s.HasPrefix("test", "te"))
    p("HasSuffix: ", s.HasSuffix("test", "st"))
    p("Index:     ", s.Index("test", "e"))
    p("Join:      ", s.Join([]string{"a", "b"}, "-"))
    p("Repeat:    ", s.Repeat("a", 5))
    p("Replace:   ", s.Replace("foo", "o", "0", -1))
    p("Replace:   ", s.Replace("foo", "o", "0", 1))
    p("Split:     ", s.Split("a-b-c-d-e", "-"))
    p("ToLower:   ", s.ToLower("TEST"))
    p("ToUpper:   ", s.ToUpper("test"))
}
```

```bash
Contains:   true
Count:      2
HasPrefix:  true
HasSuffix:  true
Index:      1
Join:       a-b
Repeat:     aaaaa
Replace:    f00
Replace:    f0o
Split:      [a b c d e]
ToLower:    test
ToUpper:    TEST
```

## String Formatting
#String-Formatting 

Some examples of common string formatting tasks. for detail explanation [String Formatting](https://gobyexample.com/string-formatting)

```go
package main

import (
	"fmt"
	"os"
)

type point struct {
	x, y int
}

func main() {

	p := point{1, 2}
	fmt.Printf("struct1: %v\n", p) // struct1: {1 2}

	fmt.Printf("struct2: %+v\n", p) // struct2: {x:1 y:2}

	fmt.Printf("struct3: %#v\n", p) // struct3: main.point{x:1, y:2}

	fmt.Printf("type: %T\n", p) // type: main.point

	fmt.Printf("bool: %t\n", true) // bool: true

	fmt.Printf("int: %d\n", 123) // int: 123

	fmt.Printf("bin: %b\n", 14) // bin: 1110

	fmt.Printf("char: %c\n", 33) // char: !

	fmt.Printf("hex: %x\n", 456) // hex: 1c8

	fmt.Printf("float1: %f\n", 78.9) // float1: 78.900000

	fmt.Printf("float2: %e\n", 123400000.0) // float2: 1.234000e+08
	fmt.Printf("float3: %E\n", 123400000.0) // float3: 1.234000E+08

	fmt.Printf("str1: %s\n", "\"string\"") // str1: "string"

	fmt.Printf("str2: %q\n", "\"string\"") // str2: "\"string\""

	fmt.Printf("str3: %x\n", "hex this") // str3: 6865782074686973

	fmt.Printf("pointer: %p\n", &p) // pointer: 0x...

	fmt.Printf("width1: |%6d|%6d|\n", 12, 345) // width1: |    12|   345|

	fmt.Printf("width2: |%6.2f|%6.2f|\n", 1.2, 3.45) // width2: |  1.20|  3.45|

	fmt.Printf("width3: |%-6.2f|%-6.2f|\n", 1.2, 3.45) // width3: |1.20  |3.45  |

	fmt.Printf("width4: |%6s|%6s|\n", "foo", "b") // width4: |   foo|     b|

	fmt.Printf("width5: |%-6s|%-6s|\n", "foo", "b") // width5: |foo   |b     |

	s := fmt.Sprintf("sprintf: a %s", "string")
	fmt.Println(s) // sprintf: a string

	fmt.Fprintf(os.Stderr, "io: an %s\n", "error") // io: an error
}
```

## JSON
#Go-JSON 

Go offers built-in support for JSON encoding and decoding, including to and from built-in and custom data types.

```go
package main

import (
    "encoding/json"
    "fmt"
    "os"
)

// these structs demonstrate encoding and decoding of custom types below.

// the exported fields to be encode/decode in JSON, must start with capital letters to be exported. 
// see (2) for the usage
type response1 struct { // decoding
    Page   int
    Fruits []string
}

// tags on struct field declarations to customize the encoded JSON key names. 
// see (1) for the usage
type response2 struct { // encoding
    Page   int      `json:"page"`
    Fruits []string `json:"fruits"`
}

func main() {

    bolB, _ := json.Marshal(true)
    fmt.Println(string(bolB))

    intB, _ := json.Marshal(1)
    fmt.Println(string(intB))

    fltB, _ := json.Marshal(2.34)
    fmt.Println(string(fltB))

    strB, _ := json.Marshal("gopher")
    fmt.Println(string(strB))

    slcD := []string{"apple", "peach", "pear"}
    slcB, _ := json.Marshal(slcD)
    fmt.Println(string(slcB))

    mapD := map[string]int{"apple": 5, "lettuce": 7}
    mapB, _ := json.Marshal(mapD)
    fmt.Println(string(mapB))

// (2) The JSON package can automatically encode your custom data types. It will only include exported fields in the encoded output and will by default use those names as the JSON keys. 
// {"Page":1,"Fruits":["apple","peach","pear"]}
    res1D := &response1{
        Page:   1,
        Fruits: []string{"apple", "peach", "pear"}}
    res1B, _ := json.Marshal(res1D)
    fmt.Println(string(res1B))

// (1) {"page":1,"fruits":["apple","peach","pear"]}
    res2D := &response2{
        Page:   1,
        Fruits: []string{"apple", "peach", "pear"}}
    res2B, _ := json.Marshal(res2D)
    fmt.Println(string(res2B))
    
// Now let’s look at decoding JSON data into Go values. Here’s an example for a generic data structure.

    byt := []byte(`{"num":6.13,"strs":["a","b"]}`)

// provide a variable where the JSON package can put the decoded data.
    var dat map[string]interface{} // hold a map of strings to arbitrary data types.

//  decoding, and a check for associated errors.
    if err := json.Unmarshal(byt, &dat); err != nil {
        panic(err)
    }
    fmt.Println(dat)

// In order to use the values in the decoded map, we’ll need to convert them to their appropriate type
    num := dat["num"].(float64)
    fmt.Println(num)

// Accessing nested data requires a series of conversions.
    strs := dat["strs"].([]interface{})
    str1 := strs[0].(string)
    fmt.Println(str1)

// We can also decode JSON into custom data types. This has the advantages of adding additional type-safety
    str := `{"page": 1, "fruits": ["apple", "peach"]}`
    res := response2{}
    json.Unmarshal([]byte(str), &res)
    fmt.Println(res)
    fmt.Println(res.Fruits[0])

// We can also stream JSON encodings directly to `os.Writer`s like `os.Stdout` or even HTTP response bodies.
    enc := json.NewEncoder(os.Stdout) // JSON encoder 
    d := map[string]int{"apple": 5, "lettuce": 7}
    enc.Encode(d)
}
```

```bash
true
1
2.34
"gopher"
["apple","peach","pear"]
{"apple":5,"lettuce":7}
{"Page":1,"Fruits":["apple","peach","pear"]}
{"page":1,"fruits":["apple","peach","pear"]}
map[num:6.13 strs:[a b]]
6.13
a
{1 [apple peach]}
apple
{"apple":5,"lettuce":7}
```

## Time
#Time 
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    p := fmt.Println

    now := time.Now()
    p(now)

    then := time.Date(
        2009, 11, 17, 20, 34, 58, 651387237, time.UTC)
    p(then)

    p(then.Year())
    p(then.Month())
    p(then.Day())
    p(then.Hour())
    p(then.Minute())
    p(then.Second())
    p(then.Nanosecond())
    p(then.Location())

    p(then.Weekday())

// These methods compare two times following
    p(then.Before(now))
    p(then.After(now))
    p(then.Equal(now))

// The `Sub` methods returns a `Duration` representing the interval between two times.
    diff := now.Sub(then)
    p(diff)

    p(diff.Hours())
    p(diff.Minutes())
    p(diff.Seconds())
    p(diff.Nanoseconds())

// You can use `Add` to advance a time by a given duration, or with a `-` to move backwards by a duration.
    p(then.Add(diff))
    p(then.Add(-diff))
}
```

```bash
2012-10-31 15:50:13.793654 +0000 UTC
2009-11-17 20:34:58.651387237 +0000 UTC
2009
November
17
20
34
58
651387237
UTC
Tuesday
true
false
false
25891h15m15.142266763s
25891.25420618521
1.5534752523711128e+06
9.320851514226677e+07
93208515142266763
2012-10-31 15:50:13.793654 +0000 UTC
2006-12-05 01:19:43.509120474 +0000 UTC
```


## Time-Formatting/Parsing 
#Time-Formatting-via-Parsing 

Go supports time formatting and parsing via pattern-based layouts. More detail [Time-Formatting/Parsing](https://gobyexample.com/time-formatting-parsing)

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    p := fmt.Println

    t := time.Now()
    p(t.Format(time.RFC3339))

    t1, e := time.Parse(
        time.RFC3339,
        "2012-11-01T22:08:41+00:00")
    p(t1)

    p(t.Format("3:04PM"))
    p(t.Format("Mon Jan _2 15:04:05 2006"))
    p(t.Format("2006-01-02T15:04:05.999999-07:00"))
    form := "3 04 PM"
    t2, e := time.Parse(form, "8 41 PM")
    p(t2)

    fmt.Printf("%d-%02d-%02dT%02d:%02d:%02d-00:00\n",
        t.Year(), t.Month(), t.Day(),
        t.Hour(), t.Minute(), t.Second())

    ansic := "Mon Jan _2 15:04:05 2006"
    _, e = time.Parse(ansic, "8:41PM")
    p(e)
}
```

```bash
2014-04-15T18:00:15-07:00
2012-11-01 22:08:41 +0000 +0000
6:00PM
Tue Apr 15 18:00:15 2014
2014-04-15T18:00:15.161182-07:00
0000-01-01 20:41:00 +0000 UTC
2014-04-15T18:00:15-00:00
parsing time "8:41PM" as "Mon Jan _2 15:04:05 2006": ...
```