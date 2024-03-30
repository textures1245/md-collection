[[Go Practical Examples]] 

Knowledge required
[[Goroutine]]
[[Go Channels]]
[[Select]]
[[Concurrently]]

Src: [main](https://github.com/textures1245/go-practical-examples/blob/main/main.go), [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/concurrently/index.go) 

Assignment 
- Implement a producer-consumer pattern using goroutines and channels, where one goroutine produces data, and another consumes it.
- Write a program that concurrently fetches data from multiple URLs and combines the results.
- Create a program that simulates a simple web crawler using goroutines and channels, where each goroutine fetches and processes a URL.

```go
package concurrently

import (
	"bufio"
	"fmt"
	"net/http"
	"sync/atomic"
)

// - Implement a producer-consumer pattern using goroutines and channels, where one goroutine produces data, and another consumes it.
type ProduceService struct {
	C int32
}

func Worker(id int, job <-chan func() int32, res chan<- int32) {
	for j := range job {

		doJob := j
		go func() {
			result := doJob()
			res <- result
		}()
	}
}

func (p *ProduceService) Produce() int32 {
	atomic.AddInt32(&p.C, 1)
	return p.C
}

func (p *ProduceService) Consume() int32 {
	atomic.AddInt32(&p.C, -1)
	return p.C
}

// - Write a program that concurrently fetches data from multiple URLs and combines the results.
type FetchService struct {
	Urls []string
	C    int32
}

func (f *FetchService) Fetch(url string, lineLimiter int) string {

	resp, err := http.Get(url)
	if err != nil {
		panic(err)
	}

	defer resp.Body.Close()

	var context string
	scanner := bufio.NewScanner(resp.Body)
	for i := 0; scanner.Scan() && i < lineLimiter; i++ {
		context = scanner.Text()
	}

	if err := scanner.Err(); err != nil {
		panic(err)
	}

	return context

}

// - Create a program that simulates a simple web crawler using goroutines and channels, where each goroutine fetches and processes a URL.

type CrawService struct {
}

type Fetcher interface {
	// Fetch returns the body of URL and
	// a slice of URLs found on that page.
	Fetch(url string) (body string, urls []string, err error)
}

// Crawl uses fetcher to recursively crawl
// pages starting with url, to a maximum of depth.
func (c CrawService) Crawl(url string, depth int, fetcher Fetcher) string {
	// TODO: Don't fetch the same URL twice.
	// TODO: Fetch URLs in parallel.
	// This implementation doesn't do either:
	if depth <= 0 {
		return ""
	}
	body, urls, err := fetcher.Fetch(url)
	if err != nil {
		fmt.Println(err)
		return ""
	}
	res := fmt.Sprintf("found: %s %q\n", url, body)
	for _, u := range urls {
		c.Crawl(u, depth-1, fetcher)
	}
	return res
}

// fakeFetcher is Fetcher that returns canned results.
type FakeFetcher map[string]*FakeResult

type FakeResult struct {
	Body string
	Urls []string
}

func (f FakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.Body, res.Urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher is a populated fakeFetcher.
```

```go
func concurrentlyTask() {

	// - Implement a producer-consumer pattern using goroutines and channels, where one goroutine produces data, and another consumes it.
	service1 := concurrently.ProduceService{C: 0}
	const numJobs = 10
	jobs1 := make(chan func() int32, numJobs)
	res1 := make(chan int32, numJobs)

	// create workers
	for w := 1; w <= 3; w++ {
		go concurrently.Worker(w, jobs1, res1)
	}

	for j := 1; j <= numJobs; j++ {
		if rand.Intn(2) == 1 {
			jobs1 <- service1.Produce
		} else {
			jobs1 <- service1.Consume
		}
	}
	close(jobs1)

	for a := 1; a <= numJobs; a++ {
		fmt.Println(<-res1)
	}

	fmt.Printf("Final Value: %d\n", atomic.LoadInt32(&service1.C))

	// - Write a program that concurrently fetches data from multiple URLs and combines the results.
	urls := [5]string{}
	for i := 0; i < 5; i++ {
		urls[i] = "https://www.dochord.com/chord_charts/"
	}

	// - Create a program that simulates a simple web crawler using goroutines and channels, where each goroutine fetches and processes a URL.
	service2 := concurrently.FetchService{Urls: urls[:]}
	var res2 [5]string
	var wg sync.WaitGroup

	for i, url := range service2.Urls {
		wg.Add(1)
		index := i
		go func(url string) {
			defer wg.Done()
			res2[index] = service2.Fetch(url, 1)
		}(url)
	}

	wg.Wait()
	fmt.Println(res2)

	var fetcher = concurrently.FakeFetcher{
		"https://golang.org/": &concurrently.FakeResult{
			Body: "The Go Programming Language",
			Urls: []string{
				"https://golang.org/pkg/",
				"https://golang.org/cmd/",
			},
		},
		"https://golang.org/pkg/": &concurrently.FakeResult{
			Body: "Packages",
			Urls: []string{
				"https://golang.org/",
				"https://golang.org/cmd/",
				"https://golang.org/pkg/fmt/",
				"https://golang.org/pkg/os/",
			},
		},
		"https://golang.org/pkg/fmt/": &concurrently.FakeResult{
			Body: "Package fmt",
			Urls: []string{
				"https://golang.org/",
				"https://golang.org/pkg/",
			},
		},
		"https://golang.org/pkg/os/": &concurrently.FakeResult{
			Body: "Package os",
			Urls: []string{
				"https://golang.org/",
				"https://golang.org/pkg/",
			},
		},
	}

	service3 := concurrently.CrawService{}
	res3 := make(chan string)

	for i := 1; i < 5; i++ {
		index := i
		go func() {
			res := service3.Crawl("https://golang.org/", index, fetcher)
			res3 <- res

		}()
	}
	fmt.Println(<-res3)
}
```

```terminal
-1
-2
-3
-2
-1
-2
-1
-2
-1
0
Final Value: 0
[<!DOCTYPE html> <!DOCTYPE html> <!DOCTYPE html> <!DOCTYPE html> <!DOCTYPE html>]
not found: https://golang.org/cmd/
not found: https://golang.org/cmd/
not found: https://golang.org/cmd/
found: https://golang.org/ "The Go Programming Language"
```
