# Producer Consumer Design Pattern
- Concurrency design pattern that coordinates work between two types of components.
	- Producers -> Generates data ( tasks, messages, items )
	- Consumers -> Process the data.
- A shared buffer or queue is used between them, allowing decoupling of data production from data consumption.
- Producers generate data and place it into the buffer, while consumers retrieve and process data from the buffer independently.
- This mechanism allows both to operate asynchronously at different rates without direct dependency on each other.

- ## When to Use
	- When one part of the system produces data or tasks or events and another part consumes or processes them at its own pace.
	- When we want to offload heavy work to background threads or services without blocking the main thread.
	- When multiple producers and consumers need to share limited resources safely ( thread pool, database connections, file access).
	- When the system needs to handle high throughput by scaling consumers independently from producers.

- ## Components
	- **Producer** - The component that generates data or tasks an puts them into a shared queue or buffer.
	- **Consumer** - The component that takes data or tasks from the queue and processes them.
	- **Buffer/Queue** - A shared data structure that temporarily holds produced data until it is consumed, It acts as a asynchronization point.

- ## Pros
	- Decoupling - Producers and Consumers are independently work, producers don't care who consumes the data, and consumers don't care who produces it.
	- Scalability - Multiple producers and multiple consumers can be added easily for higher throughput.
	- Concurrency Friendly - Naturally supports parallelism and efficient CPU utilization.
	- Resource Management - Buffer or Queue regulates flow, preventing consumers from being overwhelmed or producers from over producing.
	- Flexibility and Reusability - Producers and Consumers can evolve separately.

- ## Cons
	- Complexity overhead - Requires thread-safe buffers, synchronozation or channels, which adds implementation complexity.
	- Risk of Deadlocks or Starvation - Poorly designed synchronization may cause deadlocks or leave consumers waiting independently.
	- Debugging Difficulty - Concurrency issues ( race conditions, deadlocks, ordering problems ) are hard to trace and reproduce.
	- Performance overhead - Synchronization and context switching can reduce performance in low-scale scenario.

- ## Sample implementation in Golang

```go
// Producer and Consumer with channels.
package main

import (
	"fmt"
	"time"
)

// Producer function
func Producer(ch chan int) {
	for i:=1; i<=5; i++ {
		fmt.Println("Produced : ", i)
		ch <- i // send to channel ( buffer )
		time.Sleep(time.Millisecond*5000)
	}
	close(ch) // Close channel after producing
}

// Consumer function
func Consumer(ch chan int) {
	for item := range ch {
		// receives until channel is closed
		fmt.Println("Consumed : ", item)
		time.Sleep(time.Millisecond*8000)
	}
}

func main() {
	ch := make(chan int, 2) // Buffered channel act as queue
	go Producer(ch)

	Consumer(ch) // run consumer in main goroutine
}
```
