# Seperation of Concerns Principle
- This principle states that a software system should be divided into distinct sections or modules, where each section addresses a separate concerns or aspect of the applications functionality.
- A concern refers to a particular feature, responsibility or functionality within the system.
- By separating concerns, each part of the system focuses on a single aspect and minimises overlap with other parts.

- ## When to Use
	- When building layered architectures ( eg :- controller -> service -> repository )
	- When writing code that involves different responsibilities ( eg :- validation, logging, data access )
	- When we want to make code modular, testable and maintainable.
	- When working in large teams -> prevents developers from interfering with each other's modules.

- ## How can we break SoC Principle
	- Mixing business logic with database queries inside the same function.
	- Embedding UI rendering logic inside data processing functions.
	- Writing logging, error handling, and validation directly inside core logic.
	- Having a god class or monolithic function that does everything.

- ## How to apply SoC Principle
	- Use layered architecture.
		- Controller / Handler -> Handles HTTP requests.
		- Service -> Contains business logic.
		- Repository / DAO -> Handles database access.

	- Apply modular design -> separate packages for utils, models, config.
	- Use interfaces in Go to decouple modules.
	- Follow SoC with clean architecture or hexagonal architecture patterns.

- ## Pros
	- Improves maintainability ( changes in one concern don't affect others ).
	- Increases testability ( test business logic without DB ).
	- Encourages reusability of modules .
	- Helps with parallel development ( teams can work on separate layers ).

- ## Cons
	- Can introduce extra boilerplate ( more files, packages, layers ).
	- If over-applied, might lead to unnecessary complexity.
	- Small projects may feel over-engineered with strict separation.

- ## Sample implementation in golang
	- **Breaking SoC ( everything mixed in one place )**
	```go
	package main

	import (
	    "database/sql"
	    "fmt"
	    _ "github.com/lib/pq"
	)

	func main() {
	    conn, _ := sql.Open("postgres", "user=postgres dbname=test sslmode=disable")
	    rows, _ := conn.Query("SELECT name FROM users WHERE id=$1", 1)

	    var name string
	    for rows.Next() {
	        rows.Scan(&name)
	    }

	    // Bussiness Logic + DB access + Printint ( UI ) all mixed
	    fmt.Println("User name is : ", name)
	}
	```
	- **Applying SoC ( separated layers )**
	```go
	package main
	
	import (
	    "database/sql"
	    "fmt"
	    _ "github.com/lib/pq"
	)

	// Repository Layer
	type UserRepository struct {
	    db *sql.DB
	}

	func (r UserRepository) GetUserName(id int) (string, error) {
	    var name string
	    err := r.db.QueryRow("SELECT name FROM users WHERE id = $1", id).Scan(&name)

	    return name, err
	}

	// Service Layer
	type UserService struct {
	    repo UserRepository
	}

	func (s UserService) GetGreeting(id int) (string, error) {
	    name, err := s.repo.GetUserName(id)
	    if err != nil {
	        return "", err
	    }

	    return "User name is : " + name, nil
	}

	// Presentation Layer
	func main() {
	    db, _ := sql.Open("postgres", "user=postgres dbname=test sslmode=disable")

	    repo := UserRepository{db : db}
	    service := UserService{repo : repo}

	    greeting, _ := service.GetGreeting(1)

	    fmt.Println(greeting)
	}
	```
