# Strategy Design Pattern
- Behavioural design pattern that lets us define a family of algorithms, encapsulate each one, and make them interchangeable at runtime.
- This allows an object to alter its behaviour by switching between different strategies dynamically promoting flexibility and maintainability without modifying its own code.
- Means instead of hardcoding logic into a class, the class delegates the work to a chosen strategy object that implements the desired algorithm.

- ## When to Use
	- When we have multiple ways to perform a task or calculation.
	- When the behaviour of a class needs to change dynamically at runtime.
	- When we want to avoid cluttering our code with conditional logic ( like if-else or switch statements ) for every variation.

- ## Components
	- **Context** - Maintains a reference to a strategy and is configured with one at runtime. Delegates algorithm execution to the strategy object.
	- **Strategy Interface** - Specifies a contract for a family of interchangable algorithms, all concrete strategies implement this interface.
	- **Concrete Strategies** - Implement specific algorithm confirming to the strategy interface. The context can swap these to change behaviour as needed.
	- **Client** - Chooses the appropriate concrete strategy and configures it in the context.

- ## Working
	1. Client selects the strategy that fits the required behaviour and injects it into the context.
	2. When a context operation requiring the algorithm is triggered, it delegates it to the current strategy.
	3. Different strategies can be changed or extended without altering the context or client, following the Open/Closed Principle.

- ## Pros
	- Eliminates conditional logic - Replaces big if-else or switch blocks with polymorphism. Each algorithm lives in its own class.
	- Open/Closed Principle - We can add new strategies without changing existing code.
	- Flexibility at runtime - Strategies can be swapped dynamically depending on context.
	- Encapsulation of algorithms - Each algorithm is isolated -> easier to understand, maintain and test independently.
	- Reusability - Same strategies can be reused across different contexts.

- ## Cons
	- Increased number of classes - Each algorithm requires a separate class -> can lead to class explosion.
	- Client awareness - Client must know which strategy to use in a given situation.
	- Overhead of simple cases - If only a couple of algorithm exist, strategy may feel like unnecessary abstraction.
	- No control over misuse - If the wrong strategy is set the context may misbehave without obvious errors.

- ## Sample implementation in Golang

```go
package main

import "fmt"

// Strategy Interface
type PaymentStrategy interface {
	Pay(amount float64)
}

// Concrete Strategy 1
type CreditCardPayment struct {}

func (c *CreditCardPayment) Pay(amount float64) {
	fmt.Printf("Paid %.2f using credit card\n", amount)
}

// Concrete Strategy 2
type PaypalPayment struct {}

func (p *PaypalPayment) Pay(amount float64) {
	fmt.Printf("Paid %.2f using Pay Pal\n", amount)
}

// Context
type PaymentProcessor struct {
	strategy PaymentStrategy
}

func (pp *PaymentProcessor) SetStrategy(strategy PaymentStrategy) {
	pp.strategy = strategy
}

func (pp *PaymentProcessor) CheckOut(amount float64) {
	pp.strategy.Pay(amount)
}

// Client
func main() {
	processor := &PaymentProcessor{}

	processor.SetStrategy(&CreditCardPayment{})
	processor.CheckOut(1000.0)

	processor.SetStrategy(&PaypalPayment{})
	processor.CheckOut(200.0)
}
```
