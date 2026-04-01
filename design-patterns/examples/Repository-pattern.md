# Repository Design Pattern
- Let's consider a restaurat has
	1. Chef ( Service Layer ) -> Prepares food ( business logic )
	2. Waiter ( Repository ) -> Takes orders and brings food
	3. Kitchen/Storage ( Database ) -> Where food / data exists.

## Real World Example
### Without Repository Pattern
- Chef directly goes to kitchen ( chef -> Kitchen )
- Problems
	- Chef needs to know where everythingis stored
	- If kitchen layout changes -> chef needs to adapt
	- Hard to scale

### With Repository Pattern
- Chef talks only to waiter
- Benefits
	- Chef doesn't need to care how kitchen works.
	- Kitchen can change -> waiter handles it
	- Cleaner, scalable system


## Software Example
- Let us consider we have an E-Commerce app. Let us consider a scenario where the user opens product page, system fetches products.

### Without Repository Pattern
```go
func GetProduct(id int) (*Product, error) {
    quert := "SELECT * FROM products WHERE id = $1"
    // DB logic here
}
```
- Problems
	- Business logic + DB logic
	- Hard to test
	- Hard to change DB

### With Repository Pattern
- Step1 :- Define Repository ( Waiter )
	```go
	type ProductRepository interface {
	    GetByID(id int) (*Produt, error)
	}
	```
- Step 2 :- Service ( Chef )
	```go
	type ProductService struct {
	    resp ProductRepository
	}

	func (s *ProductService) GetProduct(id int) (*Process, error) {
	    return s.resp.GetByID(id)
	}
	```
	- Service doesn't know
		- SQL
		- PostgreSQL
		- Redis
		
- Step 3 :- Implementation ( Kitchen Interaction )
	```go
	type productRepo struct {
	    db *sql.DB
	}

	func (r *productRepo) GetByID(id int) (*Product, error) {
	    query := "SELECT * FROM products WHERE id = $1"
	    // DB Logic
	}
	```
