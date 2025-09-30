# Singleton Design Pattern
- Creational Design Pattern that ensures that a class has only one single instance throughout an application and provides a global point of access to that instance.
- This pattern is typically used when exactly one object is needed to coordinate actions across the system.

- ## Key Principles
	- **Single Instance** :- Only one instance of the class is created and maintained.
	- **Global Access** :- Provides a controlled, global point of access to the single instance.
	- **Private Constructor** :- The constructor is made private to prevent external classes from instantiating the class directly.
	- **Lazy/Eager Initialization** :- The instance can be created eagerly at load time or lazily when first created.
	- **Thread Safety** :- In multithreaded environments, the pattern ensures that multiple threads do not create multiple instances concurrently.

- ## When to Use
	- When a single instance must control access to a shared resources.
	- For managing shared configurations, caches, logging, thread pools or device connections.
	- When centralized control or coordination is needed.
	- Inshort for following
		- Database connection pool
		- Logger
		- Configuration loader.
		- Cache Manager.

-  ## Pros
	- Ensures a single instance of class and provides a global point of access to it.
	- Only one object is created, which can be particular beneficial for resources heavy classes.
	- Provides a way to maintain global state within a application.
	- Supports lazy loading, where instance is only created when its first needed.
	- Guarantees that every object in the application uses the same global variable.

- ## Cons
	- Violates the Single Responsibility Principle :
		- This pattern solves two problems at the same time.
			- Control of Instance creation
			- Global Point of Access.
	- In multithreaded environments, special care must be taken to implement singletons correctly to avoid race conditions.
	- Introduces global state into an application, which might be difficult to manage.
	- Classes using the singleton can become tightly coupled to the singleton class.
	- Singleton patterns can make unit testing difficult due to the global state it introduces.

- ## Sample Implementation in Go
	- Go doesn't have classes, but we can still implement singleton pattern using structs + package level variables.
	- Go also provides ***Sync.Once*** type which makes it easy to safely initialize singleton pattern in a concurrent program.
	
	```go
	package singleton

	import (
	    "fmt"
	    "sync"
	)

	// Singleton Struct
	type Singleton struct {
	    data string
	}

	var instacne *Singleton
	var once sync.Once

	// Get Instance returns the single instance
	func GetInstance() *Singleton {
	    once.Do(func() {
	        fmt.Println("Creating single instance")
	        instance = &Singleton{data : "Initial data"}
	    })
	    return instance
	}
	```
