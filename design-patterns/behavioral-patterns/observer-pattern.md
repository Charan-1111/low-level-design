# Observer Design Pattern
- Behavioural design pattern that defines a one-to-many dependency between objects.
- When one object ( called the subject ) changes its state, all its dependent objects ( called observers ) are notified automatically and updated automatically.
- This pattern promotes loose coupling by keeping the subject and observers independent ; the subject only knows that observers implement a common interface, without knowing their concrete types.

- ## When to Use
	- When we have multiple parts of the system that needs to react to a change in one central component.
	- When we want to decouple the publisher of data from the subscribers who react to it.
	- When we need a dynamic, event-driven communication model without hardcoding who is listening to whom.

- ## Components
	- **Subject** - Maintains a list of observers and provides methods to register, unregister, and notify them of state changes.
	- **Observer** - Defines an interface with an update method that concrete observers implement to receive state change notifications.
	- **Concrete Subject** - Implements the subject interface, holds the actual state, and notifies observes when the state changes.s
	- **Concrete Observer** - Implements the observer interface, maintains a reference to the subject, and updates itself based on state changes.

- ## Working
	1. Observers register themselves to the subject.
	2. When the subject's state changes, it calls a notify method.
	3. The notify method invokes the update method on all registered observers.
	4. Observers retrive the updated data from the subject and react accordingly.

- ## Pros
	- Loose coupling - The subject doesn't need to know details about the observers, only that they follow an interface. Observers can be added or removed at runtime without changing the subject.
	- Supports Broadcast Communication - A single state change in the subject automatically notifies all interested observers. Useful when many objects need to react to the same state.
	- Scalable and Extensible - Subject and Observers can evolve independently and be reused in other contexts.
	- Promoted Real-Time Updates - Observers stay in sync with subjects state without constantly pooling.

- ## Cons
	- Unexpected update chains - A subject's update may trigger multiple observes, which in turn update other subjects, can lead to cascading updates and complexity.
	- Memory Leak Risk - If observers are not unsubscribed properly, they may stay in memory ( common issue in event-driven systems ).
	- Notification Overhead - If there are too many observers, frequent notifications can hurt performance.
	- Hard to debug - Since updates are triggered indirectly, it can be difficult to trace which subject's change caused which observer's action.
	- Order of Notification - If observers rely on update order, the pattern doesn't gurantee consistent ordering unless explicitly handled.

- ## Sample implementation in Golang

```go
package main

import "fmt"

// Observer interface
type Observer interface {
	Update(string)
}

// Subject interface
type Subject interface {
	Register(Observer)
	Unregister(Observer)
	NotifyAll()
}

// Concrete Subject
type NewsPublisher struct {
	observers []Observer
	news string
}

func (p *NewsPublisher) Register(o Observer) {
	p.observers = append(p.observers, o)
}

func (p *NewsPublisher) Unregister(o Observer) {
	for i, observer := range p.observers {
		if observer == o {
			p.observers = append(p.observers[:i], p.observers[i+1:]...)
			break
		}
	}
}

func (p *NewsPublisher) NotifyAll() {
	for _, observer := range p.observers {
		observer.Update(p.news)
	}
}


func (p *NewsPublisher) AddNews(news string) {
	p.news = news
	p.NotifyAll()
}

// Concrete Observer
type Subscriber struct {
	name string
}

func (s *Subscriber) Update(news string) {
	fmt.Printf("%s received news : %s\n", s.name, news)
}

// Client Code
func main() {
	publisher := &NewsPublisher{}

	sub1 := &Subscriber{name: "Alice"}
	sub2 := &Subscriber{name: "Bob"}

	publisher.Register(sub1)
	publisher.Register(sub2)

	publisher.AddNews("New Go release is out")

	publisher.Unregister(sub1)

	publisher.AddNews("Observer pattern explained")
}
```
