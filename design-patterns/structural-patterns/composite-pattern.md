# Composite Design Pattern
- Structural Design Pattern that lets us treat individual objects and groups of objects uniformly.
- Especially useful when we have a tree-like structure ( hierarchies ) such a files and folders, employees and departments, UI component etc.

- ## When to Use
	- When we need to represent part-whole hierarchies.
	- When we want to perform operations on both leaf nodes and composite nodes in a consistent way.
	- When we want to avoid writing special-case logic to distinguish between single and grouped objects.

- ## Components
	- **Component** - An abstract interface or class that defines common operations for both leaf and composite objects.
	- **Leaf** - Represents individual objects in the hierarchy that do not have children, implements the component interface directly and performs actual work.
	- **Composite** - Represents complex objects that can have children ( both leaves and other composites ) and implements child-related operations by delegating requests to these child components.
	- **Client** - Interacts with components solely through the component interface, so it does not need to distinguish between leaf and composite objects.

- ## Working
	1. Composites manages a list of child components, allowing creation of tree structures.
	2. Operations requested by clients on composites are propagated recursively down to child components.
	3. Leaves handle requests directly, while composites handle or forward requests to their children.
	4. This pattern eliminates the need to manage objects differently on their type ( leaf or composite )

- ## Pros
	- Uniformity - Treats individual objects ( leaves ) and groups of objects ( composite ) the same way.
	- Simplifies client code - No need for if/else checks like "is this is a file or a folder" ?
	- Scalability - Adding new leaf or composite types is easy without chaning client code.
	- Hierarchial Structure made easy - Perfect for trees ( file systems, org charts, UI Components, menus )
	- Extensibility - We can combine objects into more complex structures dynamically at runtime.

- ## Cons
	- Can make design overly generic - Sometimes we lose type safety because everything is treated as a component.
	- Harder to restrict behaviour.
	- Debugging complexity - In large trees, if can be tricky to debug or trace operations through all the nested components.
	- Performance overhead - Recursive calls over large hierarchies may impact performance.

- ## Sample implementation in Golang

```go
// File System

package main

import "fmt"

// Component interface
type FileSystemNode interface {
	Show(indentation string)
}

// Leaf
type File struct {
	name string
}

func (f *File) Show(indentation string) {
	fmt.Println(indentation + f.name)
}


// Composite
type Directory struct {
	name string
	children []FileSystemNode
}

func (d *Directory) Add(node FileSystemNode) {
	d.children = append(d.children, node)
}

func (d *Directory) Show(indentation string) {
	fmt.Println(indentation + d.name + "/")
	for _, child := range d.children {
		child.Show(indentation + " ")
	}
}

// Client
func main() {
	// Create leaf nodes
	file1 := &File{name : "file1.txt"}
	file2 := &File{name : "file2.txt"}
	file3 := &File{name : "file3.txt"}

	// Create composite nodes
	dir1 := &Directory{name: "Documents"}
	dir2 := &Directory{name: "Pictures"}

	// Build tree
	dir1.Add(file1)
	dir1.Add(file2)
	dir2.Add(file3)

	root := &Directory{name: "Root"}
	root.Add(dir1)
	root.Add(dir2)

	// Client just calls Show() on root
	root.Show(" ")
}
```
