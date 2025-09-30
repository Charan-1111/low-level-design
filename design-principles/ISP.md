# Interface Segregation Principle ( ISP )
- This principle states that no client should be forced to depend on the methods, that they don't use.
- This means, interfaces should be split into smaller, more focused ones so that implementing classes only need to provide relevant functionality.
- **Eg** :- A printer interface shouldn't force every device to also implement Scan() and Fax() methods if they don't support them.

- ## When to Use
	- When we notice interfaces with too many unrelated methods.
	- When different clients need different subsets of behaviour.
	- When implementing an interface requires empty or dummy methods.
	- When we want to decouple functionality for flexibility.

- ## How can we break ISP
	- By creating "fat" interfaces with multiple unrelated responsibilities.
	- By forcing structs to implement methods they don't need.
	- By making changes to an interface that unnecessarily ripple across all implementations.

- ## How to apply ISP
	- Split large interfaces into smaller, focused ones.
	- Design interfaces aroung client needs rather than system convenience.
	- Apply Single Responsibility Principle to interfaces.
	- Use composition of interfaces ( Go allows embedding small interfaces into bigger ones )

- ## Pros
	- Reduces unnecessary dependencies.
	- Improves flexibility ( implement only what's needed )
	- Easier to extend with new behaviours.
	- Promotes clean, maintainable interfaces.

- ## Cons
	- Can result in many small interfaces, which might feel verbose.
	- Requires discipline in design to avoid fragmentation.
	- In small or simple projects, splitting interfaces may feel like extra work.

- ## Sample implementation in golang
	- **Violates ISP - fat interfaces**
	```go
	package main

	import "fmt"

	// Too many responsibilities
	type MultiFunctionDevice interface {
	    Print(doc string)
	    Scan(doc string)
	    Fax(doc string)
	}

	// Old printer, only prints, but forced to implement all

	type OldPrinter struct{}

	func (OldPrinter) Print(doc string) {
	    fmt.Println("Printing : ", doc)
	}

	func (OldPrinter) Scan(doc string) {
	    // Not supported
	}

	func (OldPrinter) Fax(doc string) {
	    // Not supported
	}
	```
	- **Applying ISP - smaller focused interfaces**
	```go
	package main

	import "fmt"

	// Segregated interfaces
	type Printer interface {
	    Print(doc string)
	}

	type Scanner interface{
	    Scan(doc string)
	}

	type Faxxer interface {
	    Fax(doc string)
	}

	// Devices implements only what they need
	type OldPrinter struct{}

	func (OldPrinter) Print(doc string) {
	    fmt.Println("Printint : ", doc)
	}

	type ModernPrinter struct{}

	func (ModernPrinter) Print(doc string) {
	    fmt.Println("Printint : ", doc)
	}

	func (ModernPrinter) Scan(doc string) {
	    fmt.Println("Scanning : ", doc)
	}

	func (ModernPrinter) Fax(doc string) {
	    fmt.Println("Faxing : ", doc)
	}

	func main() {
	    var p Printer = OldPrinter{}
	    p.Print("Report")

	    var mp Printer = ModernPrinter{}
	    mp.Print("Invoice")

	    var s Scanner = ModernPrinter{}
	    s.Scan("Contract")
	}
	```
