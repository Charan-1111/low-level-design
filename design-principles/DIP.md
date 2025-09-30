# Dependency Inversion Principle ( DIP )
- This principle states that
	- High-level modules should not depend on low-level modules, Instead they both should depend on abstractions.
	- Abstractions should not depend on details. Details should depend on abstractions.
- In short :- Depend on interfaces ( contracts ), not concrete implementations.

- ## When to Use
	- When we want our system to be flexible, testable and maintainable.
	- When high-level business logic should remain unaffected.
	- When we foresee the need to swap implementations,
		- **Eg** :- different databases, payment gateway, or caching strategies.

- ## How can we break DIP
	- Diredctly instantiating dependencies inside high-level modules ( db := &MySQLDatabase())
	- Tightly coupling business logic with frameworks libraries or infrastructure.
	- Using concrete types instead of interfaces for dependencies.
	- Writing classes or modules that cannot be tested without their dependencies.

- ## How to apply DIP
	- Introduce interfaces or abstractions to define contracts.
	- High-level modules depend on those interfaces.
	- Use dependency injection ( constructor injection, setter injection, or frameworks ) to provide implementations.
	- In Go, we commonly use interfaces + struct composition.

- ## Pros
	- Increases flexibility - Easy to swap dependencies.
	- Enhances testability - Can mock dependencies easily .
	- Improves maintainability - High-level logic doesn't change when low-level details changes.
	- Encourages clean architecture - clear separation of business logic and infrastructure.

- ## Cons
	- Can lead to over-engineering if used where not needed.
	- Requires extra abstractions ( interfaces ), which may add complexity in small projects.
	- Might be harder for beginners to grasp initially.

-  ## Sample implementation in golang
	```go
	package main

	import "fmt"

	// Abstraction
	type Database interface {
	    Save(data string)
	}

	// low-level module
	type MySQLDatabase struct {}

	func (db *MySQLDatabase) Save(data string) {
	    fmt.Println("Saving to MySQL : ", data)
	}

	// another low-level module
	type MongoDB struct {}

	func (db *MongoDB) Save(data string) {
	    fmt.Println("Saving to Mongo : ", data)
	}

	// High-level module
	type UserService struct {
	    db Database // Depends on abstraction, not concrete type
	}

	func (u *UserService) RegisterUser(name string) {
	    fmt.Println("Registering User : ", name)
	    u.db.Save(name)
	}

	func main() {
	    // Injecting MySQL

	    mysql := &MySQLDatabase{}
	    service1 := &UserService{db : mysql}
	    service1.RegisterUser("Alice")

	    // Injecting Mongo
	    mongo := &MongoDB{}
	    service2 := &UserService{db : mongo}
	    service2.RegisterUser("Bob")
	}
	```
