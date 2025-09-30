# Open-Closed Principle ( OCP )
- It states that software entities such as class, modules or functions should be "open for extension, but closed for modification"
- This means
	- We should be able to add new functionality ( open for extension ).
	- Without changing existing, tested, and stable code ( closed for modification )
- This is commonly achieved through abstractions, interfaces, and techniques like inheritance, delegation or composition.
- ## When to Use
	- When requirements are likely to evolve ( eg :- adding new strategies, payment methods, file formats)
	- When we want to avoid breaking existing features.
	- When designing frameworks or libraries where we expect others to extend behaviour.
	- When we see frequent if/else or switch statements that grow as new features are added.

- ## How can we break OCP
	- Modifying, existing, stable code every time a new requirement comes in.
	- Writing long if/else or switch chains to handle multiple cases.
	- Hardcoding new behaviour directly inside existing classes.

- ## How to apply OCP
	- Use interfaces and abstractions to allow new behaviour without touching old code.
	- Apply polymorphism instead of branching ( if/switch )
	- Follow dependency inversion ( depend on abstraction, not concretions )
	- Refactor code to make it pluggable ( strategy pattern, decorator pattern)

- ## Pros
	- Reduces risk of breaking existing code.
	- Makes systems more scalable and flexible.
	- Encourages reuse of stable code.
	- Supports plug-and-play design.

- ## Cons
	- Can introduce extra abstractions ( more interfaces, more files )
	- Might be overkill for small or simple projects.
	- Requires careful design upfront ( too much generalisation can hurt readability )

- ## Sample implementation in golang
	- **Violates OCP ( modify code for every new payment method )**
	```go
	package main

	import "fmt"

	type PaymentService struct{}

	func (p PaymentService) Pay(method string, amount float64) {
	    if method == "credit" {
	        fmt.Println("Paid ", amount, " using credit card")
	    } else if method == "paypal" {
	        fmt.Println("Paid ", amount, " using PayPal")
	    } else if method == "upi" {
	        fmt.Println("Paid ", amount, " using UPI")
	    }

	    // Each new method requires modifying this function
	}

	func main() {
	    service := PaymentService{}
	    service.Pay("credit", 100)
	    service.Pay("upi", 150)
	}
	```
	- **Apply OCP ( extend via interfaces, no modification needed )**
	```go
	package main

	import "fmt"

	// Abstraction
	type PaymentMethod interface {
	    Pay(amount float64)
	}

	// Concrete implementations
	type CreditCard struct{}

	func (CreditCard) Pay(amount float64) {
	    fmt.Println("Paid ", amount, " using credit card")
	}

	type PayPal struct{}

	func (PayPal) Pay(amount float64) {
	    fmt.Println("Paid ", amount, " using Pay Pal")
	}

	type UPI struct{}

	func (UPI ) Pay(amount float64) {
	    fmt.Println("Paid ", amount, " using UPI")
	}

	// Service depends on abstraction
	type PaymentService struct {
	    method PaymentMethod
	}

	func (p PaymentService) Process(amount float64) {
	    p.method.Pay(amount)
	}

	func main() {
	    service := PaymentService{method : CreditCard{}}

	    service.Process(100)

	    service = PaymentService{method : UPI{}}
	    service.Process(150)

	    // Adding a new method = just implement PaymentMethod
	}
	```
