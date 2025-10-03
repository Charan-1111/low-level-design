# Bridge Design Pattern
- Structural Design Pattern that decouples an abstraction from its implementation, so that the two can vary independently.
- Instead of binding an abstraction to one implementation at compile time, we can create a bridge that allows us to mix and match them freely.
- This pattern splits a class into two separate hierarchies.
	- One for the abstraction
	- One for the implementation.
- These two hierarchies are bridged via composition not inheritance, allowing use to mix and match independently.

- ## When to Use
	- When we have classes that can be extended in multiple orthogonal dimensions.
		- Eg :- Shape vs rendering technology, UI Control vs platform.
	- When we want to avoid a deep inheritance hierarchy that multiples combinations of features.
	- When we need to combine multiple variations of behaviour or implementation at the runtime.

- ## Components
	- **Abstraction** - Defines the high-level interface and holds a reference to an implementor object. It delegates actual work to the implementor.
	- **Refined Abstraction** - Extends the abstraction interface with additional functionality, still using the implementor for low-level operations.
	- **Implementor** - Defines the interface for implementation classes, doesn't have to match the abstraction interface.
	- **Concrete Implementor** - Provides the specific implementation of the implementor interface.

- ## Working
	1. Client interacts only with the abstraction.
	2. Abstraction contains a reference to an implementation.
	3. Calls to abstraction methods are forwarded to the implementor, allowing the abstraction and implementation to change and extend independently.
	4. This structure is ideal when both the abstraction and implementation may evolve or multiply over time, avoiding the class explosion of creating a subclass for every combination.

- ## Pros
	- Decouples abstraction from implementation - Both can evolve independently without affecting each other.
	- Avoids class explosion - Instead of creating every combination, we separate abstraction and implementation and bridge them.
	- Improves flexibility - We can choose or switch implementations at runtime.
	- Follows Open/Closed Principle - Add new abstractions or implementations without changing existing code.
	- Better Scalability - Words well in large systems where abstractions and implementations change frequently.

- ## Cons
	- Increased Complexity - We are introducing two parallel hierarchies ( abstractions and implementations ).
	- Harder initial design - Requires upfront thinking to identify the right abstraction or implementations.
	- Indirection overhead - Calls go through an extra layer ( abstraction -> implementation ), which adds a small performance and readability cost.

- ## Sample implementation in Golang

```go
// Remote & TV

package main

import "fmt"

// Implementor
type TV interface {
	On()
	Off()
	SetChannel(channel int)
}

// Concrete implementor
type SonyTV struct{}

func (s *SonyTV) On() {
	fmt.Println("Sony TV is ON")
}

func (s *SonyTV) Off() {
	fmt.Println("Sony TV is OFF")
}

func (s *SonyTV) SetChannel(channel int) {
	fmt.Printf("Sony TV set to channel %d\n", channel)
}

type SamsungTV struct{}

func (s *SamsungTV) On() {
	fmt.Println("Samsung TV is ON")
}

func (s *SamsungTV) Off() {
	fmt.Println("Samsung TV is OFF")
}

func (s *SamsungTV) SetChannel(channel int) {
	fmt.Printf("Samsung TV set to channel %d\n", channel)
}

// Abstraction
type RemoteControl struct {
	tv TV
}

func (r *RemoteControl) On() {
	r.tv.On()
}

func (r *RemoteControl) Off() {
	r.tv.Off()
}

// Refined Abstraction
type AdvancedRemote struct{
	RemoteControl
}

func (a *AdvancedRemote) SetChannel(channel int) {
	a.tv.SetChannel(channel)
}

// Client
func main() {
	sony := &SonyTV{}
	samsung := &SamsungTV{}

	basicRemote := &RemoteControl{tv : sony}
	advancedRemote := &AdvancedRemote{
		RemoteControl {
			tv : samsung,
		},
	}

	basicRemote.On()
	basicRemote.Off()

	advancedRemote.On()
	advancedRemote.SetChannel(5)
	advancedRemote.Off()
}
```
