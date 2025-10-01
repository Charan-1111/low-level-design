# Abstract Factory Design Pattern
- Creational Design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- In short it is a factory of factories that allows clients to create group of related objects seamlessly, without knowing their specific implementations.

- ## When to Use
	- Families of Related objects - when we need to create a set of related objects that should work together.
	- Consistency Matters - when we want to enforce consistency between products in the same family.
	- Switching Between Variants - when our app should be able to switch entire families at run time.
	- Decoupling from concrete implementations - clients only dependent on abstract interfaces, not specific classes.
	- Cross-Platform Systems - when we need to support multiple platforms or environments.
	- Abstract factory pattern encapsulates object creation into factory interfaces.

- ## Components
	- Abstract Factory - Defines an interface for creating abstract products.
	- Concrete Factories - Implement the abstract factory interface to create concrete products specific to a family.
	- Abstract Products - Declares interfaces for a set of related products.
	- Concrete Products - The actual implementations of the abstracts products

- ## Pros
	- Enforces consistency - prevents mixing of objects from different families.
	- Encapsulation of Families - Groups related objects together.
	- Decoupling from concrete classes - Client code only depends on interfaces, not implementations.
	- Ease of switching variants - Switching entire product families is straight forward.
	- Open / Closed Principle.

- ## Cons
	- Complexity Overhead - More interfaces, more factory classes.
	- Harder to add new products - If we add a new product type, we must update all factories.
	- Indirection can hide logic - Client doesn't know which concrete class is actually being used.
	- Rigid structure.

- ## Sample implementation in Go
```go
/*
- UI ToolKit example
- Windows UI ( Windows Button + Windows Checkbox )
- Mac UI ( Mac Button + Mac Checkbox)

- Abstract factory will help us create related objects ( Button + Checkbox ) while keeping them consistent
*/

// Step 1 :-  Define product interface

package main

import "fmt"

// Product A : Button
type Button interface {
    Paint()
}

// Product B : Checkbox
type Checkbox interface {
    Paint()
}

// Step 2 :- Implement concrete products

// Windows products
type WinButton struct{}

func (WinButton) Paint() {
    fmt.Println("Rendering win buttons")
}

type WinCheckbox struct {}

func (WinCheckbox) Paint() {
    fmt.Println("Rendering win checkbox")
}

// Mac products

type MacButton struct {}

func (MacButton) Paint() {
    fmt.Println("Rendering mac button")
}

type MacCheckbox struct {}

func (MacCheckbox) Paint() {
    fmt.Println("Rendering mac checkbox")
}

// Step 3 :- Define abstract factory

// Abstract factory

type UIFactory itnerface {
    CreateButton() Button
    CreateCheckbox() Checkbox
}

// Step 4 :- Implement concrete factories

// windows factory

type WinFactory struct {}

func (WinFactory) CreateButton() Button {
    return WinButton{}
}

func (WinFactory) CreateCheckbox() Checkbox {
    return WinCheckbox{}
}

// Mac factory
type MacFactory struct {}

func (MacFactory) CreateButton() Button {
    return MacButton{}
}

func (MacFactory) CreateCheckbox() Checkbox {
    return MacCheckbox{}
}

// Step 5 :- Client Code
func main() {
    var factory UIFactory

    // suppose the environment is Mac
    factory = MacFactory{}

    button := factory.CreateButton()
    checkbox := factory.CreateCheckbox()

    button.Paint()
    checkbox.Paint()

    // Switch to windows environment

    factory = WinFactory{}

    button = factory.CreateButton()
    checkbox = factory.CreateCheckbox()

    button.Paint()
    checkbox.paint()
}
```
