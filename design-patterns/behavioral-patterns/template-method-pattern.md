# Template Method Design Pattern
- Behavioural design pattern that defines the skeleton of an algorithm in a base ( abstract ) class, but lets subclasses override specific steps of the algorithm without changing its overall structure.
- This pattern promotes code reuse and enforces a fixed sequence of execution, letting subclasses tailor parts of the process to their needs.

- ## When to Use
	- When we have a well-defined sequence of steps to perform a task.
	- When some parts of the process are shared across all implementations.
	- When we want to allow subclasses to customize specific steps without rewriting the whole algorithm.

- ## Components
	- **Abstract Class** - Contains the template method, which defines the algorithm's fixed sequence of steps, some steps are implemented in the abstract class, which others are declared abstract or as hooks for subclasses to override.
	- **Template Method** - A method marked as final or non-override, calling various abstract or concrete steps in a defined order.
	- **Abstract or Hook Methods** - Placeholder methods allowing subclasses to provide custom behaviour or extend the algorithm.
	- **Concrete Subclasses** - Implement or override specific steps as needed while inheriting the overall algorithm structure.

- ## Working
	1. The abstract base class outlines the process via the template method.
	2. Subclasses implement the variable parts of the algorithm by overriding abstract or hook methods.
	3. This ensures the sequence of operations remains consistent while allowing step-specific customizations.

- ## Pros
	- Code reuse - Common algorithm steps are defined once in the base class and reused by subclasses.
	- Consistency - Guarantees all subclasses follow the same overall process or algorithm.
	- Flexibility for customization - Subclasses can override only the parts they need, without rewriting the whole algorithm.
	- Encourages Inversion of Control - The base class controls the workflow, subclasses just fill in the details.
	- Simplifies client code - Client's doesn't need to know the algorithm details, they just call the template method.

- ## Cons
	- Inheritance based - Subclasses are tightly coupled to the base class, which makes it less flexible than composition.
	- Class explosion - If we have many variations of behaviour, we may end up with a large hirearchy of subclasses.
	- Hard to maintain with too many subclasses - Any change in the base class can impact all subclasses.
	- Less runtime flexibility - Algorithm steps are fixed at compile time, switching behaviours dynamically is not as easy as with the strategy pattern.

- ## Sample implementation in Golang

```go
// making different types of beverages
package main

import "fmt"

// Abstract Class
type Beverage interface {
	BoilWater()
	Brew()
	PourInCup()
	AddCondiments()
	PrepareRecipe()
}

// Template ( provides the skeleton )
type BaseBeverage struct {}

func (b *BaseBeverage) BoilWater() {
	fmt.Println("Boiling Water")
}

func (b *BaseBeverage) PourInCup() {
	fmt.Println("Poueing into CUP")
}

func (b *BaseBeverage) PrepareRecipe(bev Beverage) {
	bev.BoilWater()
	bev.Brew()
	bev.PourInCup()
	bev.AddCondiments()
}

// Concrete Class - Tea
type Tea struct {
	BaseBeverage
}

func (t *Tea) Brew() {
	fmt.Println("Steeping the tea")
}

func (t *Tea) AddCondiments() {
	fmt.Println("Adding sugar and milk")
}

// Client
func main() {
	tea := &Tea{}
	tea.PrepareRecipe(tea)
}
```
