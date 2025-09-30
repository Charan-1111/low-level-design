# Factory Method Design Pattern
- Creational Design pattern that provides a way to create objects without exposing the creation logic to the client.
- Instead of directly calling the constructor, client uses a factory method to create objects.
- This makes system loosely coupled and extensible and helps in encapsulation.
- Factory method allows subclasses to override this method to create objects of specific type.

- ## Types of Factory Patterns
1. **Simple Factory ( Static Factory )** -> A single factory class decides which object to create.
2. **Factory Method** -> Each subclass decides which object to create.
3. **Abstract Factory** -> A factory of factories ( used when dealing with families of related objects )

- ## Key Concepts
	- The pattern declares a factory method in a super class or interface.
	- Subclasses override this factory method to instantiate specific concrete products.
	- The client class the factory method to get objects without knowing which specific subclass will be instantiated.
	- It relies on inheritance to delegate object creation to subclasses.

- ## When to Use
	- When a class cannot anticipate the class of object it needs to create.
	- When subclasses are expected to specify the object types they instantiate.
	- To localize the knowledge of which helper subclass is delegated for object creation.
	- When object creation logic is complex or needs to vary based on the conditions or configuration.
	- Want to follow Open/Closed principle.

- ## Components
	- **Creator** -> Abstract class or interface declaring the factory method.
	- **Concrete Creator** -> Subclasses implementing the factory method to produce concrete products.
	- **Product** -> Abstract interface or class for the objects created.
	- **Concrete Product** -> Actual class implementing the product interface.

- ## Pros
	- Encapsulation of object creation -> Client don't need to know how the objects are created.
	- Decoupling -> Client depends only on an interface or abstract type, not the concrete class.
	- Single Responsibility Principle ( SRP ) -> Object creation logic is moved to a dedicated place, instead of scatterd across  the program.
	- Open/Closed Principle ( OCP ) -> Easy to add new products without modifying client code.
	- Centralized object creation -> If object construction requires complex setup, it is easier to manage inside a factory.

- ## Cons
	- More code and complexity -> For simple object creation a factory adds unnecessary abstraction.
	- Harder to trace -> Since client calls the factory, it's not obvious which concrete class is being used -> makes debugging harder.
	- Factory can become bloated -> If we put too many types inside a single factory it violates SRP
	- May hides too much.

- ## Sample implementation in Go
```go
// Payment system

// Step 1. Define the product interface
package main 

import "fmt"

// Product interface
type PaymentMethod interface {
    Pay(amount float64)
}

// Step2 :- Implement Concrete products

// Concrete product credit card
type CreditCard struct{}

func (c CreditCard) Pay(amount float64) {
    fmt.Printf("Paid %.2f using Credit Card\n", amount)
}

// Concrete product : PayPal
type PayPal struct {}

func (p PayPal) Pay(amount float64) {
    fmt.Printf("Paid %.2f using PayPal\n", amount)
}

// Step 3 :- Create Factory Method

// Factory Method
func PaymentFactory(method string) PaymentMethod {
    switch method {
    case "credit":
        return CreditCard{}
    case "paypal":
        return PayPal{}
    default:
        return nil
    }
}

// Step 4 :- Client code
func main() {
    p1 := PaymentFactory("credit")
    p2 := PaymentFactory("paupal")

    p1.Pay(100.50)
    p2.Pay(250.00)
}
```
