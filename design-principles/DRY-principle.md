# Don't Repeat Yourself ( DRY ) Principle
- This principle states that every piece of knowledge or logic should have a single, unambiguous, authoritative representation.
- It aims to eliminate duplication of logic, constants, or processes to make code more maintainable and less error-prone.
- Whenever the same concept appears in more than one place, we introduce redundancy. Redundancy make the system harder to maintain and more error prone.
- ## When to Use
	- When we see the same piece of code ( repeated ) across multiple functions, files or services.
	- When business rules are duplicated in different modules.
	- When a change in logic requires updating multiple place.
	- When constants, queries, or configs are repeated.
	- When reusability can reduce bugs and maintenance costs.

- ## How can we break DRY principle
	- Copying logic into multiple methods or classes instead of extracting.
	- Duplicating validation rules across different services.
	- Repeating constants instead of keeping them in a config file.
	- Writing the same database queries multiple times in different repositories.
	- Creating near-identical utility functions with slight variations.

- ## How can we apply DRY principle
	- Abstract common logic into helper functions or services.
	- Use constants or config files instead of hardcoding.
	- Create reusable components for UI or repeated workflows.
	- Centralize business rules in domain services or utility layers.
	- Refactor duplicate queries into a repository or DAO layer.

- ## Pros
	- Improves maintainability ( changes in one place only ).
	- Reduces bugs from inconsistent updates.
	- Promotes reusability and cleaner architecture.
	- Makes the codebase more readable and organized.
- ## Cons
	- Over-abstraction can lead to complex, hard-to-follow code.
	- May introduce tight coupling if not carefully designed.
	- Can slow development if applied prematurely to one-off scripts.
	
- ## Sample implementation in golang
- **Breaking DRY Principle**
```go
package main

import "fmt"

func areaCircle(radius float64) float64 {
    return 3.14159 * radius * radius
}

func printCircleArea(radius float64) {
    area := 3.14159 * radius * radius
    fmt.Println("Area of the circle is : ", area)
}

func main() {
    fmt.Println(areaCircle(5))
    printCircleArea(5)
}
```
- **Applying DRY Principle**
```go
package main

import "fmt"

const pi = 3.14159 // Centralized constant

func areaCircle(radius float64) float64 {
    return 3.14159 * radius * radius
}

func printCircleArea(radius float64) {
    fmt.Println("Area of the circle is : ", areaCircle(radius))
}

func main() {
    printCircleArea(5)
}
```
