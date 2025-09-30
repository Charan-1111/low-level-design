# Law of Demeter Principle
- Law of Demeter states that a module, class or function should have limited knowledge about other modules.
- It should only communicate with
	- Itself
	- Its direct components ( its own fields or properties )
	- Objects passed as arguments
	- Objects it creates internally

- ## When to Use
	- When designing object-oriented systems ( Go uses composition, so LoD applies via interfaces and structs )
	- When code is forming a long chains of calls ( eg :- a.b.c.d -> indicates that too much knowledge of internals )
	- When modules or classes are becoming tightly coupled.
	- When we want to make our system flexible and maintainable .

- ## How can we break LoD Principle
	- Chaining multiple method calls across objects ( order.Customer.Address.City ) .
	- Allowing one object to directly manipulate another object's internals .
	- Passing around objects just to reach into their fields .
	- Violating encapsulation by exposing deep object structures .

- ## How to Apply LoD Principle
	- Hide internal details -> expose higher - level methods instead of raw data .
	- Provide delegation methods so clients don't chain calls deeply .
	- Use interfaces in Go to reduce coupling.
	- Think of it as :- "Tell, don't ask" -> ask an object to perform an action, rather than fetching data and doing it ourself.

- ## Pros
	- Reduces tight coupling between classes or modules.
	- Improved encapsulation ( hide details )
	- Increases maintainability -> changes in one class won't ripple through others.
	- Leads to clearer, higher-level APIs

- ## Cons
	- Can result in more boilerplate methods ( delegation methods )
	- If over-applied, may reduce transparency and make debugging harder.
	- Sometimes a direct call is simpler and more efficient.

- ## Sample implementation in golang
	- **Breaking LoD ( long call chains )**
	```go
	package main

	import "fmt"

	type Address struct {
	    City string
	}

	type Customer struct {
	    Address Address
	}

	type Order struct {
	    Customer Customer
	}


	func main() {
	    order := Order{Customer: { Address: {City : "Mumbai"}}}

	    // Violating LoD -> Reaching Deep internals
	    fmt.Println("City : ", order.Customer.Address.City)
	}
	```
	- **Applying LoD ( Limit Knowledge )**
	```go
	package main

	import "fmt"

	type Address struct {
	    City string
	}

	type Customer struct {
	    Address Address
	}

	type Order struct {
	    Customer Customer
	}

	// Delegate method hides internal structure

	func (o Order) GetCustomerCity() string {
	    return o.Customer.Address.City
	}

	func main() {
	    order := Order{Customer: { Address: {City : "Mumbai"}}}

	    fmt.Println("City : ", order.GetCustomerCity())

	    // cleaner, less knowledge of internals
	}
	```
