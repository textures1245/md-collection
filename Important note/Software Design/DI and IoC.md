[[Design Pattern Overview]]

- **Inversion of Control (IoC)** is a broad principle that shifts the control of object creation and dependency management from the application code to a framework or container.
    
- **Dependency Injection (DI)** is a specific implementation of IoC where dependencies are "injected" into a class rather than being instantiated within the class itself.
    

In simple terms:

- **IoC** is a concept where a framework takes over the control of object creation and lifecycle
```ts
import "reflect-metadata";
import { injectable, inject, Container } from "inversify";

@injectable()
class Database {
  connect() {
    console.log("Connected to database via IoC container");
  }
}

@injectable()
class UserService {
  private db: Database;

  constructor(@inject(Database) db: Database) {
    this.db = db;
  }

  getUser() {
    this.db.connect();
    console.log("Fetching user data...");
  }
}

// IoC Container Setup
const container = new Container();
container.bind(Database).toSelf();
container.bind(UserService).toSelf();

// Resolve dependencies automatically
const userService = container.get(UserService);
userService.getUser();

```
- **DI** is a pattern that implements IoC by injecting dependencies into a class rather than having the class create them.
.
```ts
class Database {
  connect() {
    console.log("Connected to database");
  }
}

class UserService {
  private db: Database;

  constructor(db: Database) {
    this.db = db; // Injected dependency
  }

  getUser() {
    this.db.connect();
    console.log("Fetching user data...");
  }
}

// Now we control which Database instance is injected
const dbInstance = new Database();
const userService = new UserService(dbInstance);
userService.getUser();
```