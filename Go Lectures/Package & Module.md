#Go-Concept #Go-Basic #Go-Package [[Go Lectures]]

	In the Go Lang "Package" had important role for running and build for application. and Essential for organizing code into reusable units, promoting modularity and maintainability. 
	
	Every Go program starts running in the `main package`, which is used for executable programs While `other` are packages that used as `Modules` for using in main package. 

- Before we begin we should know important keywords for knowing how go running and scanning working file
	- **GOROOT**:  Go configuration that will be controlling how go should work by following the configuration such a go version, go module etc. you can see the go environment `go env`  for detail. your **GOROOT** should be set on `local` folder for example (`/usr/local/go`)
	- **GOPATH**: Path that will be scanning go files/projects. you **GOPATH** should be set on your **go** folder system for example (`/Users/phakh/go/`)

```snippet
(for macOs only)
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin 
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Copy these file-paths then save on shell profile, here in case we used `bash` shell then we acessing by typing `open ~/.bash_profile` in `bash` shell then save the config.

```merm
graph LR
    A[Start] --> B{Project Setup}
    B -->|Set GOPATH| C[Project Constructor]
    B -->|Version Control Setup| C
    C --> D{Exporting Utilities}
    D -->|Package Name| E[Using Module Package]
    D -->|Variable Name| E
    D -->|Function Name| E
    E --> |Import package path| F[End]
```

1. Project setup
	- Starting with the work project directory. we want to work on. by default we should considering starting our project in `go` root system folder by
		- Set **GOPATH** to **go root system folder**
	- For using with **version control** we should starting our project following this path `/Users/phakh/go/src/github.com/[github_username]/[project_name]
	- Remember when setting the project. In the project constructor, every folder must have `go.mod` to inside. For specify module relative path for Go and IDE can be reconizged the file constructor. 
```dirtree
root in 

- /example
	- /src
		- /nested_folder
			- index.go
			- go.mod
		- main.go
		- go.mod
	- go.mod
```

	  
1. Project Constructor 
	- In the project. we split 2 type of packages
		- **main package**: the executable package
		- **util packages**: packages that will work as modules.  
		  
2. Exporting utilities
	- In **util packages** we can exporting our function and variable by following these rules
		- **The package** must have the same name as the directory name. 
		- **The Variable** name that will be given to people who import our Package to use must have a capital first name. 
		- **The Function** name that will be given to people who import our Package to use must have a capital letter.
```go
[FILE_NAME=generic]

// must have the same name as the directory name
package generic // <-- package name

// exporting with Capital Letter
var export = "something"

func MapKeys[K comparable, V any](m map[K]V) []K {
	r := make([]K, 0, len(m))
	for k := range m {
		r = append(r, k)
	}
	return r
}
```

4. Using Module Package 
`projectFolder=/Users/phakh/go/src/github.com/textures1245/example`
```dirtree
root in 

- /example
	- /generic
		- generic.go
	- main.go
	- go.mod
```
	Directery View structor
	
- Using `import` keyword following **the module package path** in file that we want to use 
- When using **module package** starting with **package name** then accessing to the utils 

```go
package main

import (
	"fmt"
	"github.com/textures1245/example/generic" // <-- module package path
)

func main() {
	var m = map[int]string{1: "2", 2: "4", 4: "8"}

	fmt.Println("keys:", generic.MapKeys(m)) // <<-- using module package 

	_ = generic.MapKeys[int, string](m) //  <<-- using module package 

	lst := generic.List[int]{} //  <<-- using module package 
	lst.Push(10)
	lst.Push(13)
	lst.Push(23)
	fmt.Println("list:", lst.GetAll())
}

```
