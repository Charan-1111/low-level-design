# Liskov Substitution Principle ( LSP )
- This principle states that the objects of a super class should be replaceable with objects of its subclasses without breaking the application's behaviour.
- In other words, if S it a subtype of T, then the objects of type T should be replaceable with the objects of type S, without altering correctness.
- **Eg** :- If square extends Rectangle, we should be able to use a square anywhere a Rectangle is expected.

- ## When to Use
	- When designing inheritance hierarchies ( or in Go, interface implementations )
	- When we want to ensure polymorphism works correctly.
	- When we want to guarantee subtypes don't violate contracts defined by interfaces or base classes.

- ## How can we break LSP
	-	Subclass changes expected behaviour of parent.
	-	Violating method contracts ( Pre conditions, post conditions, invariants )
	-	Throwing unexpected errors in subclass methods.

- ## How to apply LSP
	- Ensure subclasses truly follow the behaviour of their parent.
	- Prefer interfaces ( in Go ) to define behaviour contracts.
	- Avoid inheritance when "is-a" relationship is not valid ( use composition instead )
	- Write tests to validate that substituting one implementation doesn't break code.

- ## Pros
	- Ensures polymorphism works as expected.
	- Makes code predictable and stable.
	- Avoids surprise bugs caused by unexpected subclass behaviour.
	- Encourages clean interface design.

- ## Cons
	- Sometimes enforcing LSP can push us towards composition instead of inheritance.
	- Requiers careful upfront design to ensure contracts are well-defined.
	- Can feel like over-engineering in small projects.

- ## Sample implementation in golang
	- **Violates LSP - Square isn't a proper Rectangle**
	```go
	package main

	import "fmt"

	type Rectangle struct {
	    width, height int
	}

	func (r *Rectangle) SetWidth(w int) {
	    r.width = w
	}

	func (r *Rectangle) SetHeight(h int) {
	    r.height = h
	}

	func (r *Rectangle) Area() int {
	    return r.width * r.height
	}

	// Square tries to extend Rectangle but breaks behaviour

	type Square struct {
	    Rectangle
	}

	func (s *Square) SetWidth(w int) {
	    s.width = w
	    s.height = w
	    // forces height = width ( unexpected )
	}

	func main() {
	    var r Rectangle
	    r.SetWidth(4)
	    r.SetHeight(5)

	    fmt.Println("Area of Rectangle : ", r.Area())

	    var sq Square

	    sq.SetWidth(4)
	    sq.SetHeight(5)

	    // Not really respected ( height gets overridden)

	    fmt.Println("Area of Square : ", sq.Area()) // wrong expectation
	}
	```
	- **Apply LSP - use interfaces**
	```go
	package main

	import "fmt"

	// Shape interface defies contract
	type Shape interface {
	    Area() int
	}

	// Rectangle follows shape contract
	type Rectangle struct {
	    width, height int
	}

	func (r Rectangle) Area() int {
	    return r.width * r.height
	}

	// Square follows shape contract
	type Square struct {
	    side int
	}

	func (sq Square) Area() int {
	    return sq.side * sq.side
	}

	func PrintArea(sh Shape) {
	    fmt.Println("Area : ", sh.Area())
	}

	func main() {
	    r := Rectangle{width : 4, height : 5}
	    s := Square {side : 4}

	    // Both can substitute for shapre without breaking behaviour

	    PrintArea(r)
	    PrintArea(s)
	}
	```
