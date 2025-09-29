# You Aren't Gonna Need It ( YAGNI ) Principle
- This principle means that we should not implement functionality until it is actually required.
- This principle encourages to avoid speculative programming ( Building features based on the anticipated future requirements that migh never materialized ).
- Because speculative programming often lead to unnecessary complexity, wasted effort, and maintainance overhead.

- ## When to Use
	- When we feel tempted to add features for hypothetical future use cases.
	- When while designing APIs or services - Implement only the endpoints needed now.
	- When refactoring - don't over-generalize for scenarios that may never occur.
	- In Agile/Iterative development - deliver only what's required for current stories.

- ## How can we break YAGNI Principle
	- Writing generic frameworks instead of solving the specific problem at hand.
	- Adding configuration options that nobody asked for.
	- Building support for multiple database engines when the app only uses one.
	- Writing complex abstractions to support possible future needs.

- ## How to apply YAGNI Principle
	- Focus on current requirements, not on the hypothetical ones.
	- Use simplest possible code that works today.
	- Add features only when a clear, immediate need arises.
	- Pair with KISS ( Keep It Simple Stupid ) and DRY for balanced design.
	- Rely on refactoring later when need actually appears.

- ## Pros
	- Keep codebase lean and focused.
	- Reduces development time by avoiding unnecessary work.
	- Lowers maintainance cost - fewer unused features to manage.
	- Improves clarity and reduces complexity.

- ## Cons
	- May require future refactoring if new needs araise.
	- Could lead to short-term focus where long-term extensibility is sacrified.
	- Sometimes anticipating future needs can save work if those needs are very likely.

- ## Sample implementation in golang
- **Breaking YAGNI ( over-engineering )**
```go'
package main

import "fmt"

// over-designed to support any shape in future.

type Shape interface {
    Area() float64
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return 3.14159 * c.Radius * c.Radius
}

// Added square even though app only needs circle right now

type Square struct {
    Side float64
}

func (s Square) Area() float64 {
    return s.Side * s.Side
}

func main() {
    c := Circle{Radius : 5}

    fmt.Println("Area of the circle : ", c.Area())
}
```

- **Applying YAGNI ( implement only what is needed )**
```go
package main

import "fmt"

func areaCircle(radius float64) float64 {
    return 3.14159 * radius * radius
}

func main() {
    fmt.Println("Area of the circle : ", areaCircle(5))
}
```
