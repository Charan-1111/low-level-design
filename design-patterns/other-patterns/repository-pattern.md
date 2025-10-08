# Repository Design Pattern
- Structural design pattern ( sometimes considered architectural ) that abstracts the data access layer of an application.
- It provides a collection-like interface to access domain objects, isolating the business logic from the undelying data sources ( like databases, APIs, or files ).

- ## When to Use
	- When we don't want our business logic ( services, controllers ) to know about database queries or SQL/ORM specifies.
	- When our app might later use multiple databases ( Postgres, MongoDB, Oracle, Redis, APIs, files,...), repositories let us swap them without changing business logic.
	- When we want to test our business logic with mocked repositories instead of connecting to a real DB.
	- When the same data access logic is needed in multiple service or controllers, repositories prevent code duplication.
	- When queries are non-trivial ( joins, filters, pagination ), putting them in repositories keeps business logic clean and focused.

- ## Components
	- **Domain Model** - Core business entities the application operates on.
	- **Repository Interface** - Defines the contract for operations on domain entities.
	- **Concrete Repository** - Implements the repository interfaces, handling data access logic with the specific data source.
	- **Client/Service Layer** - Uses the repository to perform business logic without worrying about data retrieval or persistance.

- ## Working
	1. Clients interact with repositories via interfaces requesting data or commanding changes.
	2. The repository translates these requests into specific calls to databases or data sources, managing connections, querying and object mapping.
	3. Changes in database technology or optimization occur inside the repository without impacting business logic.

- ## Pros
	- Separation of Concerns - Keeps the business logic independent of persistence logic. Services or Controllers doesn't need to know how data is stored or retrieved.
	- Testability - Repositories can be mocked or stubbed in unit tests. Makes testing business logic easier without relying on a real database.
	- Maintainability - If we switch from PostgreSQL -> MongoDB -> REST API, only the repository layer changes, not the business logic.
	- Reusability - Centralized repository code can be reused across multiple services.
	- Abstraction and Cleaner Code - Provides a simple, collection-like API ( FindById, Save, Delete ) instead of exposing raw queries.
	- Consistency - All data code is in one place reducing duplication.

- ## Cons
	- Extra Abstraction Layer - Adds complexity for small or simple applications where direct DB calls would be fine.
	- Boilerplate Code - Requires writing repository interfaces and implementations which can feel repetitive.
	- Limited Expressiveness - For very complex queries ( like reporting or analytics ), the repository might just wrap way SQL/ORM queries, offering little benefit.
	- Over engineering risks - If the project doesn't need multiple data sources or test mocks, the pattern may slowdown development.
	- Potentital Duplication with ORMs - Some ORMs ( like Django ORM, Hibernate, SQLAlchemy ) already act like repositories, so adding another repository layer can be redundant.

- ## Sample implementation in Golang

```go
// User repository with In-Memory DB
package main

import "fmt"

// Entry ( Model )
type User struct {
	ID int
	Name string
}

// Repository Interface
type UserRepository interface {
	FindByID(id int) (*User, error)
	Save (user User) error
	FindAll() ([]User, error)
}

// Concrete Repository ( In-Memory Implementation )
type InMemoryUserRepository struct {
	users map[int]User
}

func NewInMemoryUserRepository() *InMemoryUserRepository {
	return &InMemoryUserRepository{
		users : make(map[int]User),
	}
}

func (r *InMemoryUserRepository) FindByID(id int) (*User, error) {
	user, exists := r.users[id]

	if !exists {
		return nil, fmt.Errorf("User not found")
	}

	return &user, nil
}

func (r *InMemoryUserRepository) Save(user User) error {
	r.users[user.ID] = user
	return nil
}

func (r *InMemoryUserRepository) FindAll() ([]User, error) {
	users := []User{}

	for _, u := range r.users {
		users = append(users, u)
	}

	return users, nil
}

// Client
func main() {
	repo := NewInMemoryUserRepository()

	// Save Users
	repo.Save(User{ID: 1, Name: "Alice"})
	
	// Find user by ID
	user, _ := repo.FindByID(1)

	fmt.Println("Found : ", user.Name)

	// Get all Users
	all, _ := repo.FindAll()
	for _, u := range all {
		fmt.Println("User : ", u.Name)
	}
}
```
