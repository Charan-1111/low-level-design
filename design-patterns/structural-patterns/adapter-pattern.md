# Adapter Design Pattern
- Structural Design Pattern that allows incompatible interfaces to work together.
- It acts as a bridge between two objects - It converts the interfaces of a class into another interface that clients expect, enabling integration without modifying the original class.
- Adapter pattern is especially useful for incorporating third-party libraries or legacy code into a new system.

- ## When to Use
	- When we want to use an existing class but its interface does not match the one we need.
	- When we want to reuse legacy code in a modern system without modifying it.
	- When working with third-party libraries that can't be changed.

- ## Components
	- **Target Interface** - Defines the expected interface by the client.
	- **Adaptee** - Existing class with an incompatible interface.
	- **Adapter** - Implements the target interface and translated calls to the adaptee.
	- **Clients** - Interacts with the target interface and remains unaware of the adapter.

- ## Working
	1. Clients sends a request to the adapter using the target interface.
	2. Adapter translated this request into a format the adaptee can understand.
	3. The adaptee performs the action.
	4. Response goes back to the client through adapter, keeping the client unaware of the adaptee's specifics.

- ## Pros
	- Reusability of existing code - We can use old or legacy classes or third-party libraries without modifying their source.
	- Flexibility and compatibility - Allows integration of incompatible interfaces.
	- Follows Open-Closed principle - We don't modify existing classes, we extend functionality by creating adapters.
	- Better decoupling - Client only works with target interface, not tied to the concrete adaptee.

- ## Cons
	- Increased complexity - Adds an extra layer of abstraction ( adapter ), which may makes the system harder to follow.
	- Performance overhead - In some cases, adapting cals can introduce slight runtime overhead.
	- Too many adapters can clutter design
	- Not always the best long-term solution.

- ## Sample implementation in Golang
```go
/*
    Suppose we have an old printer ( Adaptee ) but our system expects a
    modern printer interfaces ( target )
*/

package main

import "fmt"

// Target interface
type Printer interface {
    PrintText(text string)
}

// Adaptee ( incompatible interface )
type OldPrinter struct {}

func (op *OldPrinter) PrintOld(text string) {
    fmt.Println("Old Printer : ", text)
}

// Adapter
type PrinterAdapter struct {
    oldPrinter *OldPrinter
}

func (pa *PrinterAdapter) PrintText(text tsring) {
    pa.oldPrinter.PrintOld(text) // translate call
}

// Client
func main() {
    old := &OldPrinter{}

    adapter := &PrinterAdapter{oldPrinter : old}

    // Client expects printer inteface
    var printer Printer = adapter
    printer.PrintText("Hello, Adapter pattern!")
}
```
