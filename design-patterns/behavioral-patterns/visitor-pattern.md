# Visitor Design Pattern
- Behavioural design pattern that let's us add new operations to a group of related objects without changing their classes.
- Instead of putting all the logic inside the objects themselves, we can separate the logic into a visitor object.
- We can achieve it by separating the algorithm from the objects, allowing new operations to be added by simply creating new visitor classes.
- This is especially useful when working with class hierarchies for which frequent new unrelated operations are required.

- ## When to Use
	- When we have a complex object structure ( like ASTs, documents, or UI elements ) that we want to perform multiple unrelated operations on.
	- When we want to add new behaviours to classes without changing their source code.
	- When we need to perform different actions depending on an objects concrete type, without resorting to a long chain of if-else or instance of checks.

- ## Components
	- **Element Interface** - Declares an accept ( visitor ) method for accepting a visitor.
	- **Concrete Elements** - Implement the element interface, representing object types in the structure. Each calls the appropriate ***visit()*** method on the visitor, passing itself.
	- **Visitor Interface** - Declares a ***visit()*** method for each element type to handle visiting logic.
	- **Concrete Visitor** - Implements the visitor interface, providing concrete logic for each element type's visit.
	- **Object Structure** - ( optional ) Provides traversal for all elements, such as lists of shapes or nodes.

- ## Working
	1. The client creates an object structure of elements.
	2. The client creates one or more concrete visitors containing specific operations.
		Eg :- Calculating area, serializing, printing.
	3. The client "applies" a visitor to all elements by calling ***accept (visitor)***; internally, each element invokes the corresponding ***visit()*** method on the visitor, possing itself for double-dispatch.

- ## Pros
	- Adds new operations easily - We can introduce new behaviour ( new visitors ) without touching the element classes.
	- Separation of concerns - Business logic ( visitors ) is kept separate from the object structure ( elements ).
	- Open/Closed Principle ( for operations ) - Elements remain closed for modifications, while operations can be extended freely.
	- Good for complex object structures - Works well when we need to run multiple different operations on the same hierarchy of elements.
	- Centralized logic - Operations that would otherwise be scattered across many classes are collected in one place.

- ## Cons
	- Hard to add new element types - If we add a new element class, we must modify every existing visitor -> breaks open/closed principle for elements.
	- Increased Complexity - Requires double dispatch ( element.Accept(visitor) + visitor.Visit(element)), which can feel unnatural in some language.
	- Tight Coupling - Elements must expose themselves to visitors via Accept() -> introduces dependency between visitors and elements.
	- Less intutive design - Not straight forward for developers unfamiliar with the pattern, can feel like over engineering for simple cases.
	- Maintenance Overhead - As the number of visitors and element types grows, keeping them in sync becomes cumbersum.

- ## Sample implementation in Golang

```go
// Let's say we have shapes ( Circle, Rectangle ) and want to add new operations like Area and Perimeter
// Without modifying the shape classes

package main

import "fmt"

// Visitor interface
type Visitor interface {
	VisitCircle(*Circle)
	VisitRectangle(*Rectangle)
}

// Element Interface
type Shape interface {
	Accept(Visitor)
}

// Concrete Elements
type Circle struct {
	radius float64
}

func (c *Circle) Accept(v Visitor) {
	v.VisitCircle(c)
}

type Rectangle struct {
	width, height float64
}

func (r *Rectangle) Accept(v Visitor) {
	v.VisitRectangle(r)
}

// Concrete Visitor 1 :- Area Calculator
type AreaVisitor struct{}

func (a *AreaVisitor) VisitCircle(c *Circle) {
	fmt.Printf("Area of Circle : %.2f\n", 3.14*c.radius*c.radius)
}

func (a *AreaVisitor) VisitRectangle(r *Rectangle) {
	fmt.Printf("Area of Rectangle : %.2f\n", r.width*r.height)
}

// Concrete Visitor 2 :- Perimeter Calculator
type PerimeterVisitor struct{}

func (p *PerimeterVisitor) VisitCircle(c *Circle) {
	fmt.Printf("Perimeter of the Circle : %.2f\n", 2*3.14*c.radius)
}

func (p *PerimeterVisitor) VisitRectangle(r *Rectangle) {
	fmt.Printf("Perimeter of the Rectangle : %.2f\n", 2*(r.width + r.height))
}

// Client
func main() {
	shapes := []Shape{
		&Circle{radius: 5},
		&Rectangle{width: 4, height: 5},
	}

	areaVisitor := &AreaVisitor{}
	perimeterVisitor := &PerimeterVisitor{}

	for _, shape := range shapes {
		shape.Accept(areaVisitor)
		shape.Accept(perimeterVisitor)
	}
}
```
