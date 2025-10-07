# Visitor Design Pattern
- Behavioural design pattern that let's us add new operations to a group of related objects without changing their classes.
- Instead of putting all the logic inside the objects themselves, we can separate the logic into a visitor object.
- We can achieve it by separating the algorithm from the objects, allowing new operations to be added by simply creating new visitor classes.
- This is especially useful when working with class hierarchies for which frequent new unrelated operations are required.

- ## When to Use
	- When we have a complex object structure ( like ASTs, documents, or UI elements ) that we want to perform multiple unrelated operations on.
	- When we want to add new behaviours to classes without changing their source code.
	- When we need to perform different actions depending on an objects concrete type, without resorting to a long chain of if-else or instance of checks.

- ## Components
	- **Element Interface** - Declares an accept ( visitor ) method for accepting a visitor.
	- **Concrete Elements** - Implement the element interface, representing object types in the structure. Each calls the appropriate ***visit()*** method on the visitor, passing itself.
	- **Visitor Interface** - Declares a ***visit()*** method for each element type to handle visiting logic.
	- **Concrete Visitor** - Implements the visitor interface, providing concrete logic for each element type's visit.
	- **Object Structure** - ( optional ) Provides traversal for all elements, such as lists of shapes or nodes.

- ## Working
	1. The client creates an object structure of elements.
	2. The client creates one or more concrete visitors containing specific operations.
		Eg :- Calculating area, serializing, printing.
	3. 
