# Proxy Design Pattern
- Structural Design Pattern that provides a placeholder or surrogate for another object to control access to it.
- Instead of accessing the real object directly, the client interacts with the proxy, which then decides whether ( and how ) to forward the request to the real object.
- It allows additional functionality such as access control, lazy initialization, logging, or caching to be performed transparently without modifying the real object.

- ## When to Use
	- Access Control -> Restrict or control access to sensitive objects.
	- Lazy Intialization -> Create heavy objects only when needed.
	- Remote Proxy -> Represent objects in a different address space ( eg :- RPC, gRPC, RMI )
	- Caching proxy -> Cache results to improve performance.
	- Logging/Monitoring Proxy -> Add logging or performance monitoring transperantly.

- ## Components
	- **Subject ( Interface )** - Common interface for Real Subject and Proxy.
	- **Real Subject** - The actual object that does the real work.
	-  **Proxy** - Controls access to Real Subject, may add extra functionality ( logging, caching, access control)
	- **Client** - Interacts with proxy as if it were RealSubject.

- ## Working
	1. Client calls methods on the proxy.
	2. Proxy decides when and if to delegate the call to the real object.
	3. Proxy can perform additional tasks like checking security permissions, delaying object creation ( lazy loading ), caching results or logging operations.
	4. This pattern provides controlled, optimized, and safe access to resource - intensive or sensitive objects.

- ## Pros
	- Access control - Restricts or manages access to sensitive objects.
	- Lazy initialization - Heavy or resource intensive objects are created only when needed.
	- Improved performance - Can add caching to avoid repeated expensive operations.
	- Adds extra functionality transparently - Logging, monitoring, security checks can be added without changing the real object.
	- Remote access made easy - Proxy can represent objects in another address space.
	- Keeps client code simple - Client always works with the subject interface, unaware of whether it's a proxy or the real object.

- ## Cons
	- Increased complexity - Adds an extra layer of abstraction, making the design harder to follow.
	- Possible performance overhead - Every call passes through the proxy, which may slow things down if not designed well.
	- Hides actual object behaviour - Debugging can be tricky because behaviour is split between proxy and the real subject.
	- Not always necessary - For simple systems, a proxy might be overkill ( YAGNI risk ).
	- Sychronization issues ( in remote proxies ) - With network - based proxies, latency, failures, or consistency issues may occur.

- ## Sample implementation in Golang

```go
// Image loading proxy example

package main

import "fmt"

// Subject Interface
type Image interface {
	Display()
}

// Real Subject
type RealImage struct {
	fileName string
}

func NewRealImage(fileName string) *RealImage {
	fmt.Println("Loading image from the disk : ", fileName)
	return &RealImage{fileName: fileName}
}

func (r *RealImage) Display() {
	fmt.Println("Displaying : ", r.fileName)
}

// Proxy
type ProxyImage struct {
	fileName string
	realImage *RealImage
}

func (p *ProxyImage) Display() {
	// Lazy Initialization

	if p.realImage == nil {
		p.realImage = NewRealImage(p.fileName)
	}
	p.realImage.Display()
}

func main() {
	var img Image = &ProxyImage{
		fileName: "photo.png",
	}

	// Image will be loaded only once
	img.Display() // Loads + Display
	img.Display() // only Displays, skip loading
}
```
