# MVC Design Pattern
- Architectural design pattern that separates an application into three inter connected components.
	- **Model** -> Manages data, logic and business logic.
	- **View** -> Handles UI and Presentation of data.
	- **Controller** -> Manages input and acts as bridge between Model and View.
- This pattern promotes organized code, easier maintenance, and separation of concerns.

- ## When to Use
	- When our app has a clear separation between data, business logic and UI.
		- Eg :- Web apps, desktop apps, mobile apps.
	- When multiple developers or teams are working on the same project and we want modularity.
	- When requirements change frequently, MVC makes it easier to modify one layer without breaking others.
	- If we want to write unit tests for business logic independently of the UI, MVC provides clean separation.
	- When the same model needs to be presented in different ways.

- ## Components
	- **Model**
		- Encapsulates applications core data and business logic.
		- Updates it's state when the controlled requests changes.
		- Can notify views ( often via Observer pattern ).
	- **View**
		- Renders the Model's data into a UI format.
		- Displays updated information when the model changes.
		- Contains no business logic - only presentation.
	- **Controller**
		- Accepts and interprets user input.
		- Updates the Model or tells the View what to render.
		- Acts as mediator between Model and View.

- ## Working
	1. The user interacts with the View ( eg :- clicks a button or submits a form ).
	2. The View forwards input to the Controller.
	3. The Controller processes the input, performs applications logic, and updates the Model.
	4. When the Model changes, it notifies the View.
	5. The View requests updated data from the Model and renders the UI.

- ## Pros
	- Separation of Concerns - Each component ( Model, View, Controller ) has a distinct role, making the system modular.
	- Maintainability - UI, business logic, and input handling can be modified independently without breaking other parts.
	- Scalability - Works well for large, complex applications where multiple teams can work in parallel on different layers.
	- Reusability
		- Views can be reused with different Models.
		- Controllers can sometimes serve multiple Views.
	- Testability - Business logic ( Model ) cab be tested separately from UI ( View ).
	- Extensibility - New features can be added more easily due to modularity.

- ## Cons
	- Complexity Overhead - For small or simple apps, MVC introduces unnecessary layers and boilerplate.
	- Steeper Learning Curve - Beginners may struggle to understand the strict separation and flow.
	- Increased Boilerplate Code - More files or classes or interfaces to manage ( Model, View, Controller for even simple features ).
	- Tight Coupling Risk - In some implementations, controllers and views can become tightly linked, reducing flexibility.
	- Performance Issues in Large Apps - Too many updates between Model-View may lead to inefficients if not carefully designed.
	- Code Navigation Overhead - Developers may have to jump across multiple files just to trace a single feature.

- ## Sample implementation in Golang

```go
// Simple MVC with Users
package main

import "fmt"

// Model
type User struct {
	Name string
	Age int
}

// View
type UserView struct {}

func (uv *UserView) Display(user User) {
	fmt.Printf("User : %s, Age : %d\n", user.Name, user.Age)
}

// Controller
type UserController struct {
	model User
	view UserView
}

func (uc *UserController) SetUser(name string, age int) {
	uc.model.Name = name
	uc.model.Age = age
}

func (uc *UserController) UpdateView() {
	uc.view.Display(uc.model)
}

// Client
func main() {
	model := User{Name: "Alice", Age: 25}

	view := UserView{}

	controller := UserController{model: model, view: view}

	// Initial Render
	controller.UpdateView()

	// Update Model Via Controller
	controller.SetUser("Bob", 30)
	controller.UpdateView()
}
```
