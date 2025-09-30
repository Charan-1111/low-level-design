# Single Responsibility Principle ( SRP )
- This principle states that a class or module or function should have one and only one reason to change.
- This means that each class or module or function in a system should focus on a single part of the functionality, encapsulating that responsibility entirely.
- ## When to Use
	- When we see a god object ( one class doing too many things )
	- When our function or class needs to change for multiple reasons.
	- When working on scalable systems, where changes should not cause ripple effects.
	- When we want testable and maintainable code.

- ## How can we break SRP
	- Adding unrelated responsibilities into a single struct or function.
	- Mixing business logic, database logic and presentation logic.
	- Writing large functions with multiple behaviours.
	- Violating Separation of Concerns ( SoC)

- ## How to apply SRP
	- Identify the concerns in our module.
	- Split into separate packages or structs if they have different reasons to change.
	- Use interfaces to abstract responsibilities.
	- Refactor large functions into smaller, focused ones.

- ## Pros
	- Easier to maintain ( changes isolated )
	- Improves readability ( clear purpose )
	- Encourages testability ( we can unit test in isolation )
	- Reduces side effects of changes.

- ## Cons
	- Can lead to many small classes or files
	- Might feel like extra boilerplate in small projects.
	- Needs discipline to avoid over-splitting responsibilities.

- ## Sample implementation in golang
	- **Violating SRP -> One struct handles DB + logic + logging**
	```go
	package main

	import (
	    "database/sql"
	    "fmt"
	    _ "github.com/lib/pq"
	)

	type UserService struct {
	    db *sql.DB
	}

	func (s UserService) CreateUser(name string) {
	    // DB logic
	    _, err := s.db.Exec("INSERT INTO users(name) VALUES ($1)", name)
	    if err != nil {
	        fmt.Println("Error inserting user : ", err)
	        return
	    }

	    // Business logic
	    fmt.Println("User created successfully : ", name)
	}
	```
	- **Applying SRP -> responsibilities split**
	```go
	package main

	import (
	    "database/sql"
	    "fmt"
	    _ "github.com/lib/pq"
	)

	// Handles only database operations
	type UserRepository struct {
	    db *sql.DB
	}

	func (r UserRepository) Create(name string) error {
	    _, err := r.db.Exec("INSERT INTO users(name) VALUES ($1)", name)

	    return error
	}

	// Handles only business logic
	type UserService struct {
	    repo UserRepository
	}

	func (s UserService) RegisterUser(name string) {
	    err := s.repo.Create(name)
	    if err != nil {
	        fmt.Println("Error creating user  : ", err)
	        return
	    }

	    fmt.Println("User registered : ", name)
	}

	// UserRepository -> DB responsibility only
	// UserService -> business logic responsibility only
	```
