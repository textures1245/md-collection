[[Go Practical Examples]] #Go-Examples 

Knowledge Required 
- [[Error  Handling]]
- [[Pointer]]
- [[Concurrently]]

Src: [main](https://github.com/textures1245/go-practical-examples/blob/main/main.go), [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/error/index.go) 

Assignment

- Create a program that uses goroutines and channels to perform a parallel calculation, such as finding the sum of elements in a large array.

- Write a function that reads data from a file and handles various errors that may occur during the process.

```go 
[PACKAGE]
type cError struct {
	msg string
	e   error
}

// passing error to custom error
func ErrorMiddleware(err error) error {
	if err != nil {
		return &cError{e: err}
	}
	return nil
}

func (cE *cError) FileHandler(err error) {

	if err != nil {
		cE.e = err
		if errors.Is(err, os.ErrNotExist) {
			cE.msg = "File not found"
		} else if errors.Is(err, os.ErrPermission) {
			cE.msg = "Permission denied"
		} else {
			cE.msg = "Unknown error"
		}
	}
}

func (cE *cError) Error() string {
	return fmt.Sprintf("Error: %s, %v", cE.msg, cE.e)
}

type File struct {
}

func (f *File) Read(fPath string) {
	dat, err := os.ReadFile(fPath)
	// extends error with custom error, so we can handle it inside error.As logic
	fileErr := ErrorMiddleware(err)
	var cE *cError

	// handle here
	if errors.As(fileErr, &cE) {
		cE.FileHandler(err)
		fmt.Println(cE.Error())
		return
	}

	fmt.Println(string(dat))

}
```

```go
[MAIN]
f := error.File{}
f.Read("test.txt")
```

```bash
Error: File not found, open test.txt: no such file or directory
```

- Implement a program that uses pointers to modify the values of variables passed to a function.
```go
type History[T any] struct {
	pos *[]T
}

type List[T any] struct {
	elems []T
	pos   []History[T]
}

func (lst *List[T]) ChangeTo(arr []T) {
	lst.pos = append(lst.pos, History[T]{pos: &arr})
	lst.elems = arr
}

func (lst *List[T]) ChangeToHistory(index int) {
	lst.elems = *lst.pos[index].pos
}

func (lst *List[T]) GetElems() string {
	return fmt.Sprintf("current elems %v \n pointer history %v ", lst.elems, lst.pos)
}
```

```go
type History[T any] struct {
	pos *[]T
}

type List[T any] struct {
	elems []T
	pos   []History[T]
}

func (lst *List[T]) ChangeTo(arr []T) {
	lst.pos = append(lst.pos, History[T]{pos: &arr})
	lst.elems = arr
}

func (lst *List[T]) ChangeToHistory(index int) {
	lst.elems = *lst.pos[index].pos
}

func (lst *List[T]) GetElems() string {
	return fmt.Sprintf("current elems %v \n pointer history %v ", lst.elems, lst.pos)
}
```

```go
lst := error.List[int]{}
lst.ChangeTo([]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10})
lst.ChangeTo([]int{1, 2, 3, 4, 5})

fmt.Println(lst.GetElems())
lst.ChangeToHistory(0)
fmt.Println(lst.GetElems())
```

```bash
current elems [1 2 3 4 5] 
 pointer history [{0x1400000e138} {0x1400000e150}] 
current elems [1 2 3 4 5 6 7 8 9 10] 
 pointer history [{0x1400000e138} {0x1400000e150}] 
```

```go
type Counter struct {
	C int32
}

func Worker[T any](id int, job <-chan func() int32, res chan<- int32) {
	for j := range job {

		doJob := j
		go func() {
			result := doJob()
			res <- result
		}()
	}
}

func (p *Counter) Sum(arr []int32) int32 {

	for _, i := range arr {
		atomic.AddInt32(&p.C, i)
	}
	return p.C
}

func Divider(divide int, num []int32) [][]int32 {
	arr := [][]int32{}

	for i := 0; i < len(num); i += divide {
		arr = append(arr, num[i:i+divide])
	}
	return arr
}
```

```go
counter := error.Counter{C: 0}

	var sample = []int32{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

	arrNum := error.Divider(5, sample)

	fmt.Println(arrNum)

	jobs1 := make(chan func() int32, len(arrNum))
	res1 := make(chan int32, len(arrNum))

	// create workers
	for w := 1; w <= 3; w++ {
		go concurrently.Worker(w, jobs1, res1)
	}

	for _, nums := range arrNum {

		// store nums to local variable for each new iteration to prevent race condition
		// when working as concurrently
		s := nums
		jobs1 <- func() int32 {
			return counter.Sum(s)
		}
	}
	close(jobs1)

	for a := 1; a <= len(arrNum); a++ {

		fmt.Println(<-res1)
	}

	fmt.Printf("Final Value: %d\n", atomic.LoadInt32(&counter.C))
```

```bash
[[1 2 3 4 5] [6 7 8 9 10] [11 12 13 14 15] [16 17 18 19 20]]
90
155
195
210
Final Value: 210
```