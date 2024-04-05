[[Go Lectures]] #go-gin #Go-HTTP-Contents 

Gin is a web framework written in [Go](https://go.dev/). It features a martini-like API with performance that is up to 40 times faster thanks to [httprouter](https://github.com/julienschmidt/httprouter). If you need performance and good productivity, you will love Gin.

**The key features of Gin are:**
- Zero allocation router
- Fast
- Middleware support
- Crash-free
- JSON validation
- Routes grouping
- Error management
- Rendering built-in
- Extendable

### Getting Gin

With [Go module](https://github.com/golang/go/wiki/Modules) support, simply add the following import

```
import "github.com/gin-gonic/gin"
```

to your code, and then `go [build|run|test]` will automatically fetch the necessary dependencies.

Otherwise, run the following Go command to install the `gin` package:

```shell
$ go get -u github.com/gin-gonic/gin
```

### Running Gin

First you need to import Gin package for using Gin, one simplest example likes the follow `example.go`:

```go
package main

import (
  "net/http"

  "github.com/gin-gonic/gin"
)

func main() {
  r := gin.Default()
  r.GET("/ping", func(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{
      "message": "pong",
    })
  })
  r.Run() // listen and serve on 0.0.0.0:8080 (for windows "localhost:8080")
}
```

And use the Go command to run the demo:

```bash
# run example.go and visit 0.0.0.0:8080/ping on browser
$ go run example.go
```

## Contents
- [Build Tags](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#build-tags)
    - [Build with json replacement](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#build-with-json-replacement)
    - [Build without `MsgPack` rendering feature](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#build-without-msgpack-rendering-feature)
- [API Examples](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#api-examples)
    - [Using GET, POST, PUT, PATCH, DELETE and OPTIONS](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#using-get-post-put-patch-delete-and-options)
    - [Parameters in path](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#parameters-in-path)
    - [Querystring parameters](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#querystring-parameters)
    - [Multipart/Urlencoded Form](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#multiparturlencoded-form)
    - [Another example: query + post form](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#another-example-query--post-form)
    - [Map as querystring or postform parameters](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#map-as-querystring-or-postform-parameters)
    - [Upload files](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#upload-files)
        - [Single file](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#single-file)
        - [Multiple files](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#multiple-files)
    - [Grouping routes](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#grouping-routes)
    - [Blank Gin without middleware by default](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#blank-gin-without-middleware-by-default)
    - [Using middleware](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#using-middleware)
    - [Custom Recovery behavior](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-recovery-behavior)
    - [How to write log file](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#how-to-write-log-file)
    - [Custom Log Format](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-log-format)
    - [Controlling Log output coloring](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#controlling-log-output-coloring)
    - [Model binding and validation](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#model-binding-and-validation)
    - [Custom Validators](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-validators)
    - [Only Bind Query String](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#only-bind-query-string)
    - [Bind Query String or Post Data](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#bind-query-string-or-post-data)
    - [Bind Uri](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#bind-uri)
    - [Bind Header](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#bind-header)
    - [Bind HTML checkboxes](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#bind-html-checkboxes)
    - [Multipart/Urlencoded binding](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#multiparturlencoded-binding)
    - [XML, JSON, YAML, TOML and ProtoBuf rendering](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#xml-json-yaml-toml-and-protobuf-rendering)
        - [SecureJSON](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#securejson)
        - [JSONP](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#jsonp)
        - [AsciiJSON](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#asciijson)
        - [PureJSON](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#purejson)
    - [Serving static files](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#serving-static-files)
    - [Serving data from file](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#serving-data-from-file)
    - [Serving data from reader](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#serving-data-from-reader)
    - [HTML rendering](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#html-rendering)
        - [Custom Template renderer](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-template-renderer)
        - [Custom Delimiters](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-delimiters)
        - [Custom Template Funcs](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-template-funcs)
    - [Multitemplate](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#multitemplate)
    - [Redirects](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#redirects)
    - [Custom Middleware](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-middleware)
    - [Using BasicAuth() middleware](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#using-basicauth-middleware)
    - [Goroutines inside a middleware](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#goroutines-inside-a-middleware)
    - [Custom HTTP configuration](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#custom-http-configuration)
    - [Support Let's Encrypt](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#support-lets-encrypt)
    - [Run multiple service using Gin](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#run-multiple-service-using-gin)
    - [Graceful shutdown or restart](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#graceful-shutdown-or-restart)
        - [Third-party packages](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#third-party-packages)
        - [Manually](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#manually)
    - [Build a single binary with templates](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#build-a-single-binary-with-templates)
    - [Bind form-data request with custom struct](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#bind-form-data-request-with-custom-struct)
    - [Try to bind body into different structs](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#try-to-bind-body-into-different-structs)
    - [Bind form-data request with custom struct and custom tag](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#bind-form-data-request-with-custom-struct-and-custom-tag)
    - [http2 server push](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#http2-server-push)
    - [Define format for the log of routes](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#define-format-for-the-log-of-routes)
    - [Set and get a cookie](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#set-and-get-a-cookie)
- [Don't trust all proxies](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#dont-trust-all-proxies)
- [Testing](https://github.com/gin-gonic/gin/blob/master/docs/doc.md#testing)