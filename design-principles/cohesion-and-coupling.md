# Coupling and Cohesion Principle
- Cohesion and coupling are fundamental software design principles that describe how modules or components in a system relate internally and externally, significantly, and quality.
- Cohesion - Measures how closely related the responsibilities of a single module or class are
	- High cohesion -> a module focuses on one well-defined task.
	- Low cohesion -> a module does many unrelated things.

- Coupling -> Measures how dependent modules are on each other
	- Low coupling -> Modules interact through clear interfaces and are mostly independent.
	- High coupling -> Modules are tightly bound and change in one affects other.

- Good design = High cohesion + Low coupling

- ## When to Use
	- When designing modular systems ( services, packages, microservices )
	- When we want to reduce ripple effects of changes.
	- When following SOLID principles, especially SRP ( Single Responsibility )
	- When refactoring god classes or large, messy modules.

- ## How can we break Cohesion & Coupling Principle
	- Low Cohesion -> One function or class handling unrelated concerns ( eg :- database + business + logic + logging )
	- High Coupling
		- One module directly depends on internal details of another.
		- Excessive use of global variables or highly linked objects.
		- Long call chains ( a.b.c.d )

- ## How to apply cohesion & coupling principle
	- Split large modules into smaller, focused units ( increase cohesion )
	- Define clear contracts ( interfaces ) between modules ( reduces coupling )
	- Use dependency injection instead of hard dependencies.
	- Follow Separation of Concerns ( SoC ).
	- Refactor code regularly to avoid god classes.

- ## Pros
	- High cohesion -> clear responsibilities, easier to maintain.
	- Low coupling -> modules are reusable and can evolve independently.
	- Improves testability ( mock dependencies )
	- Enhances scalability ( easy to split into microservices )

- ## Cons
	- Too much separation can lead to over-engineering ( too many small modules )
	- May require more boilerplate code ( interfaces, extra files )
	- Sometimes performance trade-offs ( extra layer of abstraction)

- ## Sample implementation in golang
	- **Breaking ( Low cohesion + High coupling )**
	```go
	package main

	import (
	    "database/sql"
	    "fmt"
	    _ "github.com/lib/pq"
	)

	/*
	    This serivice does DB connection, query, business logic and printing
	*/

	type UserService struct {
	    db *sql.DB
	}

	func (s UserService) ProcessUser(id int) {
	    var name string
	    _ = s.db.QueryRow("SELECT name FROM usres WHERE id = $1", id).Scan(&name)

	    fmt.Println("Processing user : ", name)
	}

	func main() {
	    db,  _ := sql.Open("postgres", "user=postgres dbname=test sslmode=disable")
	    service := UserService{db : db}

	    service.ProcessUser(1)
	}
	```
	
	- **Applying ( High cohesion + Low coupling )**
	```go
	package main

	import (
	    "database/sql"
	    "fmt"
	    _ "github.com/lib/pq"
	)

	// Repository handles only DB concerns
	type UserRepository struct {
	    db *sql.DB
	}

	func (r UserRepository) GetUserName(id int) (string, error) {
	    var name string
	    err := r.db.QueryRow("SELECT name FROM users WHERE id = $1", id).Scan(&name)
	    return name, err
	}

	// Service : handles only business logic
	type UserService struct {
	    repo UserRepository
	}

	func (s UserService) ProcessUser(id int) (string, error) {
	    return s.repo.GetUSerName(id)
	}

	func main() {
	    db, _ := sql.Open("postgres", "user=postgres dbname=test sslmode=disable")
	    repo := UserRepository{db : db}
	    service := UserService{repo : repo}

	    name, _ := service.ProcessUser(1)

	    fmt.Println("Processing user : ", name)
	}
	```
