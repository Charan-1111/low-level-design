# Command Design Pattern
- Behavioural design pattern that turns a request ( an action to perform ) into a standalone object.
- This allows us to
	- Parameterize methods with different requests.
	- Queue requests.
	- Log requests.
	- Support Undo/Redo operations.
- This pattern decouples the object that invokes the operation from the one that knows how to perform it, providing greater flexibility and extensibility in executing commands.

- ## When to Use
	- When we want to encapsulate operations as objects.
	- When we need to queue, delay or log requests.
	- When we want to support Undo/Redo functionality.
	- When we want to decouple the object that invokes an operation from the one that knows how to perform it.

- ## Components
	- **Command Interface** - Declares a method ( often execute() ) for executing an operation.
	- **Concrete Command** - Implements the command interface and defines a binding between an action and a receiver. Stores all necessary information to call the receiver methods.
	- **Receiver** - Knows how to perform the actual operation associated with the command. The command objects invokes methods on the receiver.
	- **Invoker** - Responsible for initiating commands. It maintains a reference to a command object and invokes its execute() method.
	- **Client** - Creates concrete command objects, sets their receivers, and assigns them to the invoker.

- ## Working
	1. Client creates a concrete command with a reference to the receiver and assigns it to the invoker.
	2. Invoker triggers command execution, usually through a method like execute().
	3. Command calls the appropriate action on the receiver, performing the operation.
	4. This decoupling allows commands to be queued, logged, undone or replaced at runtime with no impact on invoker or receiver logic.

- ## Pros
	- Decouples sender and receiver - The object that invokes the request ( Invoker ) is seperated from the object that performs it ( Receiver ).
	- Supports Undo/Redo operation - Since commands state, we can reverse or apply actions easily.
	- Enables request queueing and scheduling - Commands can be queued, delayed or executed asynchronously.
	- Supports logging and history - We can log executed commands and reply them later.
	- Open/Closed Principle - New commands can be added without modifying existing code.
	- Macro commands - Multiple commands can be grouped and executed as one.

- ## Cons
	- Increased number of classes - Each action requires a separate command class, which can cause class explosion.
	- Overhead for simple actions - If the action is just one line of code, wrapping it in a command object may feel like over engineering.
	- Complex Undo/Redo logic - For complex systems tracking and reversing state changes can be tricky.
	- Potential performance cost - If commands are queued or logged heavily, it may add memory and processing overhead.

- ## Sample implementation in Golang

```go
package main

import "fmt"

// Command Interface
type Command interface {
	Execute()
}

// Receiver
type Light struct{}

func (l *Light) On() {
	fmt.Println("Light is ON")
}

func (l *Light) Off() {
	fmt.Println("Light is OFF")
}

// Concrete Commands
type LightOnCommand struct {
	light *Light
}

func (c *LightOnCommand) Execute() {
	c.light.On()
}

type LightOffCommand struct {
	light *Light
}

func (c *LightOffCommand) Execute() {
	c.light.Off()
}

// Invoker
type RemoteControl struct {
	command Command
}

func (r *RemoteControl) SetCommand(c Command) {
	r.command = c
}

func (r *RemoteControl) PressButton() {
	r.command.Execute()
}

// Client
func main() {
	light := &Light{}

	lightOn := &LightOnCommand{light: light}
	lightOff := &LightOffCommand{light: light}

	remote := &RemoteControl{}

	remote.SetCommand(lightOn)
	remote.PressButton()

	remote.SetCommand(lightOff)
	remote.PressButton()
}
```
