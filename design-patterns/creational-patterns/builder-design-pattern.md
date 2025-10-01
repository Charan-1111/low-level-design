# Builder Design Pattern
- Creational Design pattern used to create complex objects step-by-step
- Instead of having a constructor or a factory with tons of parameters. Builder pattern lets us to create an object in a more readable, flexible and controlled way.

- ## When to Use
	- When there are lot of optional parameters while creating an object.
	- When we want to avoid constructor overloading.
	- When we want to avoid telescoping constructor.
	- When we want to create objects more clear and maintainable.

- ## Components
	- **Product** -> Complex object which we want to build.
	- **Builder** -> Interface or Abstract class defining the building steps.
	- **Concrete Builder** -> Implements the steps and assembles the product.
	- **Director ( Optional )** -> Controls the order of construction.
	- **Client** -> Uses the builder to get the final product.

- Builder pattern separates the construction of a complex object from its representation.
- In builder pattern.
	- The construction logic is encapsulated in a builder.
	- The final object ( the product ) is created by calling ***build()*** method
	- The object itself typically has a private or package-private constructor, forcing construction through builder.

- ## Pros
	- Step-by-Step construction - Allows building an object gradually, making it clear and flexible.
	- Readable and fluent API - using method chaining makes code cleaner and more expressive.
	- Handles complex objects - useful when an object has many fields, especially optional ones.
	- Encapsulation of construction logic - Client doesn't worry about internal building details.
	- Easy variations

- ## Cons
	- More Boilerplate code - Need extra builder struct + methods for every product, even when object creation is simple.
	- Not always necessary - For a simple object with 2 to 3 fields, a plain constructor is sufficient.
	- Can increase complexity - If over used, it makes design unnecessarily complicated for small-scale projects.
	
- ## Sample implementation in Golang
```go
// Building a House object

// Step 1 :- Define the product

package main

import "fmt"

// Product
type House struct {
    Foundation string
    Walls string
    Roof string
}

// Step 2 :- Define the Builder
type HouseBuilder struct {
    house House
}

func NewHouseBulder() *HouseBuilder {
    return &HouseBuilder{}
}

func (b *HouseBuilder) SetFoundation(foundation string) *HouseBuilder {
    b.house.Foundation = foundation
    return b
}

func (b *HouseBuilder) SetWalls(walls string) *HouseBuilder {
    b.house.Walls = walls
    return b
}

func (b *HouseBuilder) SetRoof(roof string) *HouseBuilder {
    b.house.Roof = roof
    returun b
}

func (b *HouseBuilder) Build() House {
    return b.house
}

// Step 3 :- Client Code

func main() {
    house := NewHouseBulder().SetFoundation("Concrete").SetWalls("Brick").SetRoof("Tile").Build()
}
```
