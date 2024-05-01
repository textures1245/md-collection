[[Go Lectures]] [[Clean Architecture]] #Go-Concept #Knowlaged-Category 

Ref: [Clean Architecture with Go](https://medium.com/nerd-for-tech/clean-architecture-with-golang-3fa1a1c2b6d6)

From [[Clean Architecture]] we already described. we can implemented  it as well on Go. Before deep drive to how we can adapted it, let me summarize the core design by understanding Clean Architecture with Three Layers:

## How Clean Architecture Works

Imagine you’re building a to-do list application. In a nutshell, Clean Architecture breaks down your project into layers, like the layers of an onion, where each layer has a specific purpose:

1. **Presenter Layer**: Here, how application are displayed on the user interface or how users interact with your application via API or RPC or Event.
2. **Domain Layer**: This layer contains the business logic of your application.
3. **Data Layer**: responsible for managing data storage and retrieval mechanisms, providing abstractions (such as repositories and data mappers) to interact with the data, and ensuring that the core business logic remains decoupled from the specifics of data storage technologies.

![[Pasted image 20240429150935.png]]


##  Layered Structure

You can found source code how can you implemented Clean Architecture following this SOLID Principle on [Github](https://github.com/textures1245/blog-duaaeeg-rest-api-service)

We can simply divide these layers into layered structure following by the concept of [[SOLID Principle]] 

```dirtree
root in

- go.mod
- sum.mod
- /cmd
- /internal
- /pkg
- /migrations
- /config
```

- `/cmd` -> contains file that will as entry-point app 
- `/internal` -> contains internal services that would not should be exposes on Presenter layer. Which this is how we start putting service's logical business along with Clean Architecture.
- `/pkg` -> contains package that will work along with `/internal`
- `/config` -> contains config application

And we know how layered structure works we can deep dive into each directory 
```dirtree
root in

- go.mod
- sum.mod
- /cmd
	- /api
		- main.go
	- /schedular
- /internal
	- /controller
		- http/v1 (gateway)
			- handler.go
			- route.go
	- /usecase
		- usecase.go
		- usecase_test.go
	- /repository
		- repostiory.go
		- repository_test.go
	- /entities
		- entity_noun.go (other entities go here)
	- /dtos
		- dtos_noun.go  (other dtos go here)
	- usecase.go
	- repository.go
- /pkg
	- /middlewares
	- /errors
	- /utils
	- /(other pkg goes here )
- /migrations
- /config
```

The provided layered structure follows the principles of Clean Architecture in Go, where the project is organized into different layers or components that are responsible for specific tasks. Each layer has a well-defined responsibility and communicates with other layers through interfaces or abstractions. This approach promotes code maintainability, testability, and separation of concerns. Here's an explanation of the various directories and files:

## Layer of Domains
1. `/internal/controller`:
    - This directory contains the controllers or handlers responsible for handling incoming requests and routing them to the appropriate use cases.
    - `/http/v1` (gateway):
        - This subdirectory represents the HTTP gateway or API version.
        - `handler.go`: Contains the HTTP handler functions that receive and process incoming requests.
        - `route.go`: Defines the routing rules for mapping HTTP requests to the corresponding handlers.
2. `/internal/usecase`:
    - This directory contains the use cases or business logic of the application.
    - `usecase.go`: Defines the interface and implementation for the use cases.
    - `usecase_test.go`: Contains unit tests for the use cases.
3. `/internal/repository`:
    - This directory contains the repository interfaces and implementations for interacting with data sources (e.g., databases, file systems, external APIs).
    - `repository.go`: Defines the interface for the repository.
    - `repository_test.go`: Contains unit tests for the repository implementation.
4. `/internal/entities`:
    - This directory holds the entity or domain model structs, which represent the core business objects or concepts.
    - `entity_noun.go`: Contains the definition of an entity struct (e.g., `User`, `Product`, etc.).
5. `/internal/dtos`:
    - This directory contains the Data Transfer Object (DTO) structs, which are used for transferring data between different layers or components.
    - `dtos_noun.go`: Defines the DTO structs for a specific entity or domain model (e.g., `UserDTO`, `ProductDTO`, etc.).
6. `/internal/usecase.go` and `/internal/repository.go`:
    - These files define the interfaces for the use cases and repositories, respectively.
    - They serve as a contract or abstraction layer, allowing different implementations to be swapped in and out as needed.

By following this layered structure, the Clean Architecture principles are applied:

- The `entities` layer represents the core business logic and domain models, independent of any external concerns.
- The `usecases` layer orchestrates the flow of data and coordinates the interactions between entities and repositories.
- The `repositories` layer abstracts the data access logic, allowing the application to be decoupled from specific data sources.
- The `dtos` layer facilitates data transfer between layers, providing a clear separation of concerns.
- The `controllers` layer handles incoming requests, translates them into use case invocations, and returns the appropriate responses.

This architecture promotes modularity, testability, and maintainability by separating concerns and adhering to the Dependency Inversion Principle. Each layer depends on abstractions (interfaces) from the layers below, rather than concrete implementations, enabling easier testing and swapping of implementations.

