# Facade Design Pattern
- Structural Design Pattern that provides a unified, simplified interface to a complex subsystem, making it easier for clients to interact with multiple components.
- Instead of exposing all the details of multiple classes, we provide a single unified interface ( the facade ) that clients can use easily.

- ## When to Use
	- When we want to hide the complexity of a subsystem for clients.
	- When the system contains many interdependent classes or low-level APIs.
	- When we want to provide a high-level API to make our code easier to use.
	- When working with a legacy system that's too complex for direct usage.

- ## Components
	- **Facade** - Main class that clients interact with. It offers a simple interface and delegates client requests to appropriate subsystem classes.
	- **Subsystem Classes** - These are the complex set of classes that implement the subsystem's functionality.
	- **Client** - Interacts only with the facade, not directly with the subsystem, reducing dependencies on complex parts.

- ## Working
	1. Client sends request to the facade.
	2. Facade translates these requests and delegates them to the appropriate subsystem classes.
	3. Subsystem classes execute the requests, and the facade may perform additional logic before returning results to the client.

- ## Pros
	- Simplifies a complex system - Provides a single high-level entry point.
	- Improves usability and readability - Makes API easier, to use and understand.
	- Decouples client code from subsystem - If subsystem implementation changes, client remains unaffected.
	- Encourages loose coupling - Client depends only on the facade, not on the complex internal classes.
	- Better maintainability.

- ## Cons
	- Risk of God Object - If Facade becomes too big ( handling too many responsibilities ), it can turn into a god object, that knows too much, and does too much.
	- Hide Functionality - Some advanced or specialized features of the subsystem may not be exposed through the facade.
	- Extra layer of Abstraction - Adds another layer of class in between, which might be unnecessary for very simple systems.
	- Reduced flexibility if overused - Forcing all clients to go through the Facade may restrict customization and direct fine-grained control of subsystems.

- ## Sample implementation in Golang

```go
/*
Building a Home Theater system with multiple components ( Amplifier, DVD Player, Projector, Lights)

Instead of asking client to handle all of them individually, we provide a facade.
*/

package main

import "fmt"

// Subsystems
type Amplifier struct{}

func (a *Amplifier) On() {
    fmt.Println("Amplifier On")
}

func (a *Amplifier) SetVolume(level int) {
    fmt.Printf("Volume set to %d\n", level)
}

type DVDPlayer struct {}

func (d *DVDPlayer) On() {
    fmt.Println("DVD Player on")
}

func (d *DVDPlayer) Play(movie string) {
    fmt.Printf("Playing movie : %s\n", movie)
}

type Projector struct {}

func (p *Projector) On() {
    fmt.Println("Projector On")
}

func (p *Projector) WideScreenMode() {
    fmt.Println("Projector in wide screen mode")
}

type Lights struct {}

func (l *Lights) Dim(level int) {
    fmt.Printf("Lights dimmed to %d%%\n", level)
}

// Facade
type HomeTheaterFacade struct {
    amp *Amplifier
    dvd *DVDPlayer
    projector *Projector
    lights *Lights
}

func NewHomeTheaterFacade(a *Amplifier, d *DVDPlayer, p *Projector, l *Lights) *HomeTheaterFacade {
    return &HomeTheaterFacade {
        amp : a,
        dvd : d,
        projector : p,
        lights : l,
    }
}

func (h *HomeTheaterFacade) WatchMovie(movie string) {
    fmt.Println("Get ready to watch a movie....")
    h.lights.Dim(10)
    h.projector.On()
    h.projector.WideScreenMode()
    h.amp.On()
    h.amp.SetVolume(5)
    h.dvd.On()
    h.dvd.Play(movie)
}

// Client
func main() {
    amp := &Amplifier{}
    dvd := &DVDPlayer{}
    projector := &Projector{}
    lights := &Lights{}

    homeTheater := NewHomeTheaterFacade(amp, dvd, projector, lights)

    homeTheater.WatchMovie("Inception")
}
```
