# Composing Objects Principle
- This principle also known as composition over inheritance.
- This principle suggests that instead of building complex system by inheriting behaviours from parent classes, we should compose objects by combining smaller, reusable components.
	- Inheritance -> "is-a" relationship
	- Composition -> "has-a" relationship

- ## When to Use
	- When we want to reuse behaviour without forcing a rigid inheritance hierarchy.
	- When a system needs to adapt behaviours dynamically ( flexibility )
	- When we want to loose coupling between components.
	- When building plug-and-play ( eg :- strategy pattern, decorator pattern...)

- ## How can we break this principle
	- Over using deep inheritance chains ( eg :- Animal -> Mammal -> Dog -> Police Dog -> ...)
	- Hardcoding behaviour in a single base class
	- Making child classes depend heavily on parent implementations.

- ## How to apply this principle
	- Favor interfaces + composition over inheritance.
	- Breakdown functionality into small, cohesive objects.
	- Inject dependencies via constructors or setters
	- Use Go interfaces to define expected behaviours, then compose them.

- ## Pros
	- Avoids tight coupling of inheritance hierarchies.
	- Encourages modularity and reusability.
	- Promotes flexibility ( easily swap components )
	- Works well with Go's interface system ( no inheritance )

- ## Cons
	- Can lead to more boilerplate code. ( extra interfaces and structs )
	- May become harder to trace behaviour when too many small objects are composed.
	- Requires good design discipline to avoid over-engineering.

- ## Sample implementation in golang
	- **Breaking ( using inheritance like behaviour ( not idiomatic in go))**
	```go
	// Not good in Go :- Trying to mimic inheritance
	type Animal struct{}

	func (a Animal) Speact() string {
	    return "...."
	}

	type Dog struct{
	    Animal
	}

	func (d Dog) Speak() string {
	    return "Woof!"
	}
	```
	- **Applying ( Composition with interfaces ( better approach))**
```go
package main

import "fmt"

// Define a behavior
type Speaker interface {
    Speak() string
}

// Compose Dog with Speaker behavior
type Dog struct{}

func (d Dog) Speak() string {
    return "Woof!"
}

// Compose Cat with Speaker behavior
type Cat struct{}

func (c Cat) Speak() string {
    return "Meow!"
}

// Owner has a Pet ( Composition )
type Owner struct {
    Pet Speaker
}

func main() {
    dogOwner := Owner{ Pet : Dog{} }
    catOwner := Owner{ Pet : Cat{} }

    fmt.Println("Dog owner hears : ", dogOwner.Pet.Speak())
    fmt.Println("Cat owner hears : ", catOwner.Pet.Speak())
}
```
