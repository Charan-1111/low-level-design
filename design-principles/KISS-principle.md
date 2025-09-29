# Keep It Simple Stupid ( KISS ) Principle
- This principle emphasizes that designs, solutions or systems work best when they are kept as simple as possible rather than complex.
- Core idea is to avoid unnecessary complexity ensuring that the solution achieves its goals with minimal effort and complications.
- This principle helps make code easier to understand implement, maintain and extend.

- ## When to Use
	- When designing new features or API's -> aim for clarity over cleverness.
	- When writing business logic -> avoid over-engineering.
	- When refactoring -> strip out redundant layers of abstraction.
	- When onboarding new developers -> make the system easy to understand.
	
	
- ## How can we break KISS Principle
	- Adding unnecessary abstractions, layers or design patterns.
	- Writing overly generic code for hypothetical future use cases.
	- Using complex logic when a simple condition would suffice.
	- Over-optimizing prematurely ( before bottlenecks appear ).
	- Making code "too smart" and sacrificing readability.

- ## How can we apply KISS Principle
	- Prefer readable code over clever tricks.
	- Use the simplest algorithms or structures that work.
	- Refactor large, complicated functions into smaller, clear ones.
	- Avoid over-abstraction -> only generalize when needed.
	- Stick to YAGNI ( You Aren't Gonna Need It ) along side KISS.

- ## Pros
	- Increases readability and maintainability.
	- Reduces development time and congnitive load
	- Makes onboarding new developers easier.
	- Lowers the chances of introducing bugs to complexity.

- ## Cons
	- May result in less flexibility for future requirements.
	- Sometimes the "simplest" solution isn't the most efficient.
	- Can be seen as too minimalistic in large scale enterprise systems.

- ## Sample implementation in golang
	- **Breaking KISS ( too complex )**
	```go
	package main

	import "fmt"

	/*
	    over-engineered fibonacci using recursion and memoization for small task.
	*/

	func fibonacci(n int, memo map[int]int) int {
	    if val, ok := memo[n]; ok {
	        return val
	    }

	    if n <= 1 {
	        return n
	    }

	    memo[n] = fibonacci(n-1, memo) + fibonacci(n-2, memo)

	    return memo[n]
	}

	func main() {
	    memo := make(map[int]int)
	    fmt.Println("Fibonacci ( 5 ) :  ", fibonacci(5, memo))
	}
	```
	- **Applying KISS ( simple, clean )**
	```go
	package main

	import "fmt"


	func fibonacci(n int) {
	    if n <= 1 {
	        return n
	    }

	    a, b := 0, 1
	    for i := 2 ; i <= n ; i++ {
	        a, b := b, a+b
	    }
	    return b
	}

	func main() {
	    fmt.Println("Fibonacci ( 5 ) :  ", fibonacci(5))
	}
	```
