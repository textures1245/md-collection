[[Go Practical Examples]] #Go-Examples 

Knowledge Requirement
	 [[Logging|Logging]]

Src: [main](https://github.com/textures1245/go-practical-examples/blob/main/main.go), [package](https://github.com/textures1245/go-practical-examples/blob/main/examples/logger/index.go) 

Assignment 
- Implement a custom logging library in Go with the following features:
	- **Log Levels**: Your logging library should support different log levels, such as DEBUG, INFO, WARNING, ERROR, and FATAL. Each log level should have an associated level of severity.

```go
package logger

import (
	"log/slog"
	"os"
)

// Logger Level customize
type Leveler interface {
	Level() slog.Level
}

func Level(l string) *slog.HandlerOptions {
	var LogLevelMap = map[string]slog.Level{
		"TRACE":  slog.Level(-8),
		"NOTICE": slog.Level(2),
		"FETAL":  slog.Level(12),
	}

	var lS, dExist = LogLevelMap[l]
	if !dExist {
		panic("level not found")
	}

	opts := &slog.HandlerOptions{
		Level: lS,
	}

	return opts
}

type ConsoleLogger struct {
	Db *slog.Logger
}

func NewConsoleLogger(opts ...*slog.HandlerOptions) *ConsoleLogger {

	var opt *slog.HandlerOptions

	if len(opts) > 0 {
		opt = opts[0]
	}

	return &ConsoleLogger{
		Db: slog.New(slog.NewJSONHandler(os.Stdout, opt)),
	}
}

```

```go
[MAIN]
func debugTask() {
	normal_d := logger.NewConsoleLogger()

	normal_d.Db.Error("Error")
	normal_d.Db.Info("Info")
	normal_d.Db.Debug("Debug")
	normal_d.Db.Warn("Warn")

	level := logger.Level("FETAL")
	d_with_level := logger.NewConsoleLogger(level)

	d_with_level.Db.Error("Error")
	d_with_level.Db.Info("Info")
	d_with_level.Db.Debug("Debug")
	d_with_level.Db.Warn("Warn")
}
```

```bash
{"time":"2024-04-03T15:26:49.039359+07:00","level":"ERROR","msg":"Error"}
{"time":"2024-04-03T15:26:49.039769+07:00","level":"INFO","msg":"Info"}
{"time":"2024-04-03T15:26:49.039774+07:00","level":"WARN","msg":"Warn"}
{"time":"2024-04-03T15:26:49.0398+07:00","level":"INFO+2","msg":"Notice message"}
{"time":"2024-04-03T15:26:49.03983+07:00","level":"ERROR+4","msg":"Fatal level"}
```

