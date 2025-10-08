# Chain of Responsibility Design Pattern
- Behavioural design pattern that allows us to pass a request along a chain of handlers.
- Each handler decides
	- Either process the request or
	- Pass it to the next handler in the chain.
- This pattern decouples the sender of a request from its receivers, giving multiple objects a chance to handle it.

- ## When to Use
- When a request must be handled by one of many possible handlers, and we don't
- When we want to decouple request logic from the code that processes it.
- When we want to flexibly add, remove or reorder handlers without changing the client code.

- ## Components
	- **Handler Interface or Abstract class** - Defines a method for handling requests and maintains a reference to the next handler.
	- **Concrete Handlers** - Implements the handler interface to process specific requests or pass them to the next handler if unable to handle it.
	- **Client** - Initiates the requests by sending it to the first handler in the chain.

- ## Working
	1. Client sends a request to the first handler.
	2. Each handler checks if it can handle the request.
	3. If yes, it processes the request. If not if forwards the request to the next handler.
	4. This continues until a handler processes the request or the end of chain is reached.

- ## Pros
	- Decouples sender and receiver - Client doesn't need to know which handler will process the request.
	- Flexibility in request handling - We can easily reorder, add or remove handlers without changing the client code.
	- Single Responsibility Principle - Each handler has a focused job, making code cleaner and easier to maintain.
	- Open/Closed Principle - New handlers can be introduced without modifying the existing ones.
	- Graceful fallback - If one handler can't process a request, it gets passed along until one can ( or none handle it ).

- ## Cons
	- Uncertainity in request handling - A request might go unhandled if no handler in the chain can process it.
	- Harder debugging/tracing - Since the request may pass through multiple handlers, it can be tricky to follow the flow.
	- Performance Overhead - For long chains, requests may travel through many handlers unnecessarily.
	- Execution order sensitivity - The outcome depends on the order of handlers, which might not always be obvious or intutive.
	- Potential misuse - If too many handlers are chained, it can become spaghetti flow just like too many if-else conditions.

- ## Sample implementation in Golang

```go
// Logging levels
package main

import "fmt"

// Handler interface
type Logger interface {
	SetNext(Logger)
	Log(level, message string)
}

// Base struct for chaining
type BaseLogger struct {
	next Logger
}

func (b *BaseLogger) SetNext(next Logger) {
	b.next = next
}

// Concrete Handlers
type InfoLogger struct {
	BaseLogger
}

func (i *InfoLogger) Log(level, message string) {
	if level == "INFO" {
		fmt.Println("INFO : ", message)
	} else if i.next != nil {
		i.next.Log(level, message)
	}
}

type ErrorLogger struct {
	BaseLogger
}

func (e *ErrorLogger) Log(level, message string) {
	if level == "ERROR" {
		fmt.Println("ERROR : ", message)
	} else if e.next != nil {
		e.next.Log(level, message)
	}
}

type DebugLogger struct {
	BaseLogger
}

func (d *DebugLogger) Log(level, message string) {
	if level == "DEBUG" {
		fmt.Println("DEBUG : ", message)
	} else if d.next != nil {
		d.next.Log(level, message)
	}
}

// Client
func main() {
	// Build the client : INFO, ERROR, DEBUG

	info := &InfoLogger{}
	errorLog := &ErrorLogger{}
	debug := &DebugLogger{}

	info.SetNext(debug)
	debug.SetNext(errorLog)

	// Send requests
	info.Log("INFO", "This is an info message")
	info.Log("DEBUG", "Debugging mode enabled")
	info.Log("ERROR", "Something went wrong")
	info.Log("WARNING", "Unknown log level")

	// No handler for WARNING -> it will be ignored
}
```
