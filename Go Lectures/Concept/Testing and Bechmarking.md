[[Go Lectures]] #Go-Concept #Go-Core 


Unit testing is an important part of writing principled Go programs. The testing package provides the tools we need to write unit tests and the go test command runs tests.
- Typically, the code we’re testing would be in postfix with `_test`. for example if the src code name `cal` and the test file for it would then be named `cal_test.go`.
- The function that would be test, name should have start with **Test** 
- same as when do benchmarking, name should have start with **Benchmark** 
```go
package main

import (
    "fmt"
    "testing"
)

// test func 
func IntMin(a, b int) int {
    if a < b {
        return a
    }
    return b
}

// unit test that want to test, name should have start with `Test`
// test func should have pointer testing for argument
func TestIntMinBasic(t *testing.T) {
    ans := IntMin(2, -2)
    if ans != -2 {

	// (1)
	// alert the error but stil continue run test
        t.Errorf("IntMin(2, -2) = %d; want -2", ans)
    }
}

// (2)
// we can nesting the testing arg for complex test function. 
// use `t.Run()` for enables running “subtests”
func TestIntMinTableDriven(t *testing.T) {
    var tests = []struct {
        a, b int
        want int
    }{
        {0, 1, 0},
        {1, 0, 0},
        {2, -2, -2},
        {0, -1, -1},
        {-1, 0, -1},
    }

    for _, tt := range tests {

        testname := fmt.Sprintf("%d,%d", tt.a, tt.b)
        t.Run(testname, func(t *testing.T) {
            ans := IntMin(tt.a, tt.b)
            if ans != tt.want {
                t.Errorf("got %d, want %d", ans, tt.want)
            }
        })
    }
}

// unit benchmark that want to test, name should have start with `Benchmark`
func BenchmarkIntMin(b *testing.B) {
	// increasing b.N on each run until it collects a precise measurement.
	// Typically the benchmark runs a function we’re benchmarking in a loop b.N times.
    for i := 0; i < b.N; i++ {
        IntMin(1, 2)
    }
}
```

**Note**
1. `t.Error*` will report test failures but continue executing the test. `t.Fatal*` will report test failures and stop the test immediately.
2. Writing tests can be repetitive, so it’s idiomatic to use a _table-driven style_, where test inputs and expected outputs are listed in a table and a single loop walks over them and performs the test logic.

```merm
---
title: Table-Driven Test
---
flowchart TD
    start([Start]) --> define_test_cases[Define Test Cases as a Slice of Structs]
    define_test_cases --> loop_test_cases{Loop Through Test Cases}

    loop_test_cases -- Test Case --> run_test[Run Test Function with Test Case Input]
    run_test --> evaluate_result{Evaluate Result}

    evaluate_result -- Pass --> pass_test[Mark Test as Passed]
    evaluate_result -- Fail --> fail_test[Mark Test as Failed]

    pass_test --> more_test_cases{More Test Cases?}
    fail_test --> more_test_cases{More Test Cases?}

    more_test_cases -- Yes --> loop_test_cases
    more_test_cases -- No --> report_results[Report Test Results]
    report_results --> END([End])

    style start fill:#0db9d7,stroke:#333,stroke-width:2px
    style define_test_cases fill:#ffd31d,stroke:#333,stroke-width:2px
    style loop_test_cases fill:#f9f,stroke:#333,stroke-width:4px
    style run_test fill:#b8e186,stroke:#333,stroke-width:2px
    style evaluate_result fill:#ffd31d,stroke:#333,stroke-width:2px
    style pass_test fill:#b8e186,stroke:#333,stroke-width:2px
    style fail_test fill:#e85d75,stroke:#333,stroke-width:2px
    style more_test_cases fill:#ffd31d,stroke:#333,stroke-width:2px
    style report_results fill:#b8e186,stroke:#333,stroke-width:2px
    style END fill:#0db9d7,stroke:#333,stroke-width:2px
```

```bash
// Run all tests in the current project in verbose mode.
Traiphakhs-MacBook-Air:example phakh$ go test -v
=== RUN   TestIntMinBasic
--- PASS: TestIntMinBasic (0.00s)
=== RUN   TestIntMinTableDriven
=== RUN   TestIntMinTableDriven/0,1
=== RUN   TestIntMinTableDriven/1,0
=== RUN   TestIntMinTableDriven/2,-2
=== RUN   TestIntMinTableDriven/0,-1
=== RUN   TestIntMinTableDriven/-1,0
--- PASS: TestIntMinTableDriven (0.00s)
    --- PASS: TestIntMinTableDriven/0,1 (0.00s)
    --- PASS: TestIntMinTableDriven/1,0 (0.00s)
    --- PASS: TestIntMinTableDriven/2,-2 (0.00s)
    --- PASS: TestIntMinTableDriven/0,-1 (0.00s)
    --- PASS: TestIntMinTableDriven/-1,0 (0.00s)
PASS
ok      github.com/textures1245/example 0.096s
Traiphakhs-MacBook-Air:example phakh$ go test -bench=.
goos: darwin
goarch: arm64
pkg: github.com/textures1245/example
BenchmarkIntMin-8       1000000000               0.3189 ns/op
PASS
```