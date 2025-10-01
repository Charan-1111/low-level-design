# Prototype Design Pattern
- Creational Design pattern that is used when we want to create new objects by cloning ( copying ) existing ones, instead of creating them from scratch.

- ## When to Use
	- Creating new object is expensive, time-consuming or resource-intensive.
	- When we want to avoid duplicating complex initialization logic.
	- When we need many similar objects with only few differences.

- ## Components
	- **Prototype Interface** -> Declares a clone method for copying objects.
	- **Concrete Prototype** -> Implements the clone method to return a copy of itself.
	- **Client** -> Requests new objects by cloning existing prototype objects without specifying their concrete classes.

- ## Pros
	- Efficient object creation - Avoids re-creating costly objects from the scratch.
	- Simplifies object creation logic - No need for complex constructors with many parameters.
	- Support Runtime flexibility - We can register and clone prototypes dynamically at runtime, without chaning the code.
	- Reduces Subclassing - Instead of creating many subclasses just for slightly different configuration, we clone and tweak prototype.
	- Decouples client from concrete classes - The client just calls clone(), without worrying about which concrete type is getting cloned.

- ## Cons
	- Cloning complexity - Implementing clone() can be tricy.
	- Hidden coupling - If prototype object changes internally, all clones inherit those changes.
	- Memory overhead - Cloning large, heavy objects might consume lot of memory if not handled carefully.

- ## Sample implementation in Golang
```go
// Cloning shapes Circle or Rectangle

// Step 1 :- Define the prototype interface

package main

import "fmt"

// Prototype interface
type Shape interface {
    Clone() Shape
    Draw()
}

// Step 2 :- Concrete Prototypes

// Concrete Prototype : Circle
type Circle struct {
    Radius int
    Color string
}

func (c *Circle) Clone() Shape {
    // Return a copy of circle
    copy := *c
    return copy
}

func (c *Circle) Draw() {
    fmt.Println("Drawing Circle")
}

// Concrete Prototype : Rectangle
type Rectangle struct {
    Width int
    Height int
    Color string
}

func (r *Rectangle) Clone() Shape {
    // Return a copy of rectangle
    copy := *r

    return copy
}

func (r *Rectangle) Draw() {
    fmt.Println("Drawing Rectangle")
}

// Step 3 :- Client Code

func main() {
    // Original Objects
    circle1 := &Circle{Radius : 5, Color : "Red"}
    rectangle1 := &Rectangle{Width : 5, Height : 4, Color : "Blue"}

    // Clone objects

    circle2 := circle1.Clone().(*Circle)
    rectangle2 := rectangle1.Clone()(*Rectangle)

    // Modify Clone
    circle2.Color = "Green"
}
```
