# Flyweight Design Pattern
- Structural Design Pattern that helps us reduce memory usage by sharing objects instead of creating new ones for every request.
- It is mainly used when we have a large number of similar objects that can share common ( intrinsic ) data.

- ## When to Use
	- When we need to create a large number of similar objects, but most of their data is shared or repeated.
	- Storing all object data individually would result in high memory consumption.
	- When we want to separate intrinsic data ( shared, reusable data ) from extrinsic state ( context - specific, passed in at runtime ).

- ## Components
	- **Flyweight interface** - Defines methods to operate with extrinsic state passed by the client.
	- **Concrete Flyweight** - Implements the interface and stores the intrinsic state ( shared data ).
	- **Flyweight Factory** - Manages the pool of flyweight objects, ensuring reuse by returning existing instances when available or creating new ones when necessary.
	- **Client** - Maintains and provides extrinsic state while requesting flyweights from the factory.

- ## Working
	1. When a client requests an object, the flyweight factory checks if an object with the required intrinsic state exists.
	2. If yes, the existing flyweight is returned; if not a new flyweight is created and returned.
	3. The client supplies the extrinsic state when invoking methods on the flyweight, enabling different behaviour without extra memory.

- ## Pros
	- Significant memory saving - Reduces the number of objects by sharing common ( intrinsic ) data.
	- Improved performance - Less memory usage can mean faster execution, reduced GC Pressure, and better cache utilization.
	- Centralized Control of shared state - Intrinsic state is stored in one place ( the flyweight ), which avoids redundancy and makes updates easier.
	- Scales well with large object graphs - Useful in scenarios like rendering maps, UIs, or simulations where objects differ only slightly.

- ## Cons
	- Increased complexity - We must carefully separate intrinsic and extrinsic data.
	- Clients must manage extrinsic state.
	- Reduced clarity or tracability - Since objects are shared debugging can be tricky.
	- Not useful for small - Scale problems.

- ## Sample implementation in Golang
```go
package main

import "fmt"

// Fly weight
type Character interface {
	Display(position int)
}

// Concrete fly weight
type ConcreteCharacter struct {
	Symbol string // intrinsic ( shared ) data
}

func (c *ConcreteCharacter) Display(position int) {
	// Extrinsic state : position supplied by client
	fmt.Printf("Character : %s at position %d\n", c.Symbol, position)
}

// Fly weight factory
type CharacterFactory struct {
	Pool map[string]*ConcreteCharacter
}

func NewCharacterFactory() *CharacterFactory {
	return &CharacterFactory{
		Pool : make(map[string]*ConcreteCharacter),
	}
}

func (f *CharacterFactory) GetCharacter(symbol string) *ConcreteCharacter {
	if char, exists := f.Pool[symbol]; exists {
		return char
	}

	char := &ConcreteCharacter{Symbol: symbol}
	
	f.Pool[symbol] = char
	return char
}


// Client
func main() {
	factory := NewCharacterFactory()

	text := "ABABA"

	for i, ch := range text {
		char := factory.GetCharacter(string(ch))
		char.Display(i) // extrinsic state = position
	}
}
```
