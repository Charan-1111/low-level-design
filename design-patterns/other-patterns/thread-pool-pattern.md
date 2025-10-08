# Thread Pool Design Pattern
- Concurrency design pattern that manages a pool of reusable worker threads.
- Instead of creating and destroying threads for each task ( which is expensive ), the application maintains a fixed ( or dynamic ) pool of threads that can execute tasks concurrently.
- This improves performance, reduces overhead, and controls system resource usage.

- ## When to Use
	- When out application frequently needs to execute many small or short tasks, creating a new thread each time is expensive.
	- When we want to control the number of active threads so they don't overwhelm the CPU or memory.
	- When we need to process work asynchronously without blocking the main thread.
	- When task execution is frequent and continuous, reusing threads saves the cost of repeatedly creating and destroying them.
	- When tasks are independent and can be executed in parallel to improve performance.
	- Often used together with producer-consumer: producers enqueue tasks, and a thread pool of consumers processes them.

- ## Components
	- **Worker Threads** - Threads that wait for tasks to be assigned from a queue and execute them when available.
	- **Task Queue** - A queue holding tasks submitted for execution, waiting until a thread is free.
	- **Thread Pool Manager** - Controls the creation, allocation, and lifecycle of worker threads and manages the task queue.

- ## Working
	1. When a task arrives, the thread pool assigns it to an available worker thread.
	2. If all threads are busy, the tasks waits in the queue until a thread is free.
	3. After a thread finishes running a task, if returns to the pool ready to pick up another task.
	4. The pool size is configurable to optimize resource usage and system performance.

- ## Pros
	- Improved Performance - Reuses threads instead of creating or destroying them for each task, reducing overhead.
	- Resource Management - Controls the number of concurrent threads, preventing CPU and Memory exhaustion.
	- Scalability - Can handle a large number of tasks concurrently by distributing them across available threads.
	- Reusability - Worker threads are reused for multiple tasks, improving efficiency.
	- Responsiveness - Queued tasks ensures the system remains responsive even under high load.
	- Simplifies Concurrency - Provides a centralized way to manage multiple tasks without manually handling thread creation.

- ## Cons
	- Complexity - Requires careful design for queue management, synchronization, and thread life cycle.
	- Thread starvation - Long-running tasks can block threads, causing new tasks to wait indefinitely.
	- Resource Underutilization - If the pool size is too small, system resources may remain underused.
	- Context Switching overhead - Too many threads in the pool can lead to performance loss due to frequent context switching.
	- Debugging Difficulty - Concurrency bugs ( deadlocks, race conditions ) are harder to detect and resolve.
	- Queue Overload Risk - If tasks arrive faster than they are processed the task queue can grow too large, potentially leading to memory issues.

- ## Sample implementation in Golang

```go
// Thread pool using Goroutines and Channels
package main

import (
	"fmt"
	"time"
)

// Task to execute
func worker(id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Printf("Worker %d started job %d\n", id, j)
		time.Sleep(time.Second) // Simulate work.
		fmt.Printf("Worker %d finished job %d\n", id, j)
		results <- j*2
	}
}

func main() {
	const numJobs = 5

	jobs := make(chan int, numJobs)
	results := make(chan int, numJobs)

	// Creating thread pool of 3 workers
	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}

	// Send Jobs
	for j := 1; j <= numJobs; j++ {
		jobs <- j
	}
	close(jobs)

	// Collect results
	for a := 1; a <= numJobs; a++ {
		<-results
	}
}
```
