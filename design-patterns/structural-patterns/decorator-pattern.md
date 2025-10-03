# Decorator Design Pattern
- Structural Design Pattern that allows behaviour or functionality to be added to individual objects dynamically, without affecting other objects of the same class.
- It achieves this by wrapping the original object with one or more decorator classes that modify or extend the object's behaviour at runtime.

- ## When to Use
	- When we want to extend the functionality of a class without subclassing it.
	- When we need to compose behaviours at runtime in various combinations.
	- When we wanted to avoid bloated classes filled with if-else or switch logic for optional features.

- ## Components.
	- **Component Interface** - Defines the common interface for both the original objects and decorator.
	- **Concrete Component** - Original object that needs additional functionalities.
	- **Decorator** - Abstract class implementing the component interface and holding a reference to a component object. It delegates calls to the component.
	- **Concrete Decorator** - Extends the decorator and adds a specific behaviours or responsibilities.

- ## Working
	1. Client interacts with the decorated object through the component interface.
	2. Decorator wraps the original object and can add behaviour before or after delegating to the original object.
	3. Multiple decorators can be stacked on one object to combine various behaviours dynamically.

- ## Pros
	- Adds behaviour dynamically - We can attach responsibilities to objects at runtime without modifying their code.
	- Follows open-closed principle.
	- Flexible Alternative to inheritance - Instead of creating multiple subclasses for every combination of features, we can compose decorators.
	- Combinable behaviours - Multiple decorators can be stacked in different orders, giving us many variations with fewer classes.
	- Single Responsibility Principle - Each decorator focuses on one small piece of functionality.

- ## Cons
	- Increases complexity - Too many small objects wrapping each other can make the system harder to understand and debug.
	- Order matters - Sequence of applying decorators can affect the final behaviour, which can cause subtle bugs.
	- Harder debugging and tracing - Since behaviour is spread across multiple layers, it can be difficult to trace where a specific change in behaviour is happening.
	- Can lead to runtime behaviours - If a decorator relies on some behaviour that another decorator hasn't provided yet ( order issue ), it will break.

- ## Sample implementation in Golang

```go
package  temp

import  "fmt"

// Component interface
type  Coffee  interface {
	Cost() int
	Description() string
}

// Concrete component
type  PlainCoffee  struct{}

func (p *PlainCoffee) Cost() int {
	return  50
}

func (p *PlainCoffee) Description() string {
	return  "Plain Coffee"
}

// Decorator Base
type  CoffeeDecorator  struct {
	Coffee Coffee
}

func (c *CoffeeDecorator) Cost() int {
	return c.Coffee.Cost()
}

func (c *CoffeeDecorator) Description() string {
	return c.Coffee.Description()
}

// Concrete Decorators
type  MilkDecorator  struct {
	CoffeeDecorator
}

func (m *MilkDecorator) Cost() int {
	return m.Coffee.Cost() +  10
}

func (m *MilkDecorator) Description() string {
	return m.Coffee.Description() +  ", Milk"
}

type  SugarDecorator  struct {
	CoffeeDecorator
}

func (s *SugarDecorator) Cost() int {
	return s.Coffee.Cost() +  5
}

func (s *SugarDecorator) Description() string {
	return s.Coffee.Description() +  ", Sugar"
}

// Client
func  main() {
	var coffee Coffee  =  &PlainCoffee{}

	coffee =  &MilkDecorator{
		CoffeeDecorator: CoffeeDecorator{
			Coffee: coffee,
		},
	}

	coffee =  &SugarDecorator{
			CoffeeDecorator: CoffeeDecorator{
				Coffee: coffee,
			},
		}
		
	fmt.Printf("Order : %s  \n", coffee.Description())
	fmt.Printf("Total Cost : %d\n", coffee.Cost())
}
```
