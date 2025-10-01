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
