# Blazor tutorial

##### The tutorial is divided into two parts. In the first part we will learn the basic concepts of the Blazor framework and the second part will be dedicated to the development of a kanban board.
#

Blazor is an open-source framework for building web applications from Microsoft. It allows developers to build interactive user interfaces using C# and HTML.

## Why use Blazor?
- Familiar language.

- Reusable components: Blazor supports the creation of reusable components, making development easier and more efficient.

- Server-side and client-side: Blazor enables both server-side (Blazor Server) and client-side (Blazor WebAssembly) application development.

- A wide range of tools and libraries: a wide range of tools and libraries are available for Blazor to facilitate development and extend its functionality.

- JS interoperability: a Blazor application can call JavaScript functions from .NET methods and .NET methods from JavaScript functions. These scenarios are referred to as JavaScript interoperability (JavaScript communication broker).

## Blazor application hosting models
- Blazor WebAssembly - This hosting model allows Blazor applications to run directly in the browser. The client part of the application (code written in C#) is compiled into WebAssembly, which is a binary format that can be run in modern web browsers.

- Blazor Server - In this model, the application is hosted on the server and communicates with the client using the SignalR protocol, which is a technology for real-time communication between the client and the server. The client part of the application (HTML, CSS, JavaScript) is rendered on the server side and sent to the browser as a static page. The client then communicates with the server using SignalR for interactive updates and events.

- Blazor Hybrid - allows you to combine desktop and mobile native client architectures with .NET and Blazor.

Learn more about hosting models here: https://learn.microsoft.com/cs-cz/aspnet/core/blazor/hosting-models?view=aspnetcore-8.0


## Part One

## How to get started with Blazor?
- We recommend having Visual Studio installed, but you can also develop in Visual Studio Code if you have, for example, the C# Dev Kit. However, this tutorial will focus primarily on Visual Studio.
- If you don't have Blazor installed, open the Visual Studio Installer and select the following item in it:


![asp net a web](https://github.com/denisvejrazka/GUI_Blazor/assets/112482063/2bb0b434-9f1e-4e54-9af4-2e25f81ba405)

## Components
Blazor applications are component-based. A component in Blazor is a user interface element, such as a page, dialog box, or data entry form. 

Components are usually written as a Razor page with the file extension *.razor*.

### Creating Components
To create a component, right-click on the Components folder and select *Add* and then "Razor Component".

## Basic syntax

With the component created, we can dive into the basic syntax.

### Using variables in HTML code
```
<h3>My name is @Name</h3>

@code {
	string Name = "Honza";
}

```

### IF Statements
```
@if (GenerateRandomNumber() == 2)
{
	<h3>Generated number is 2</h3>
}
else
{
	<h3>Generated number is not 2</h3>
} 

@code {
    private int randomNumber;
    private Random random = new Random();

    private int GenerateRandomNumber() => random.Next(1, 3); 
}
```

### Loops
```
@for (int i = 1; i <= 5; i++)
{
	<p style="color: green;" >Number @i</p>
}
```

## Counter
In the Components folder, create a new component *Counter.razor*

<details>
<summary>Counter</summary>

```
@rendermode InteractiveServer

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

</details>
Important here is the **@onclick** event handler, which allows us to run the IncrementCount() function on a button click.

## Task 1
Rewrite the code of the *Counter.razor* component to make the number green if it is divisible by 2 (without remainder).

<details>
<summary>Solution to Task 1</summary>

```
@rendermode InteractiveServer

<h1>Counter</h1>

@if (currentCount % 2 == 0)
{
    <p role="status" style="color: green;">Current count: @currentCount</p>
}
else
{
    <p>Current count: @currentCount</p>
}

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

</details>

#### Styling with HTML style attribute is not optimal, so one of the other options is to right click on the component, add the *"component name.razor "* item and add the *.css* file extension, now you have a *.css* file for this component in particular.

### Binding
- One-way Binding: one-way binding allows you to display data from C# to HTML. If the data in C# changes, so does the display in HTML. One-way binding is used in the direction from the data to the UI.
```
<p>Vaše jméno: @jmeno</p>

@code {
    string jmeno = "Honza";
}
```

- Two-way Binding: two-way binding allows you to display data from C# to HTML and also allows you to update the data in C# based on changes in HTML. This means that if the user changes a value in HTML, that change is propagated back to the C# code and the data is updated. The **@bind** directive is used for bidirectional binding.
```
<input type="text" @bind="jmeno" />

<p>Vaše jméno: @jmeno</p>

@code {
    string jmeno = "Honza";
}
```
Dvousměrný binding se používá pro interaktivní prvky, jako jsou formuláře.

<details>
<summary>Example</summary>
	
```
<input type="text" @bind="NewNote"/>
<button @onclick="AddNote">Přidat poznámku</button>

@foreach (var note in notes) 
{
    <p>@note</p>
}

@code {
    string NewNote = "";
    List<string> notes = new List<string>();
        
    private void AddNote() 
    {
        notes.Add(NewNote);
        NewNote = "";
    }
}
```

</details>

## Layout
A layout is a Razor component that shares tags with the components that reference it. For example, it can be a navigation bar or a footer. In short, a part of a web page that will be shared across many web page links.

In the code sample you can see a simple navigation bar that doesn't refer us anywhere, but is only used to demonstrate the Layout.

The component inherits from the *LayoutComponentBase* class, which is the base optional class for components that represent Layouts.

```
@inherits LayoutComponentBase
<nav>
    <ul>
        <li>Home</li>
        <li>About</li>
    </ul>
    <hr>
</nav>

<main> 
    @Body 
</main>
```
The ***@Body*** directive indicates the dynamic part of the site that will not remain the same.

The css for Navbar is as follows:
```
ul{
    list-style-type: none;
    display: flex;
}

li{
    margin: 10px;
    font-weight: 600;
    color: blue;
}
```

If you go to *Home.razor* now, Navbar will be there.
However, this does not verify that Navbar will remain there on the other website link. That's why we're going to create an arbitrary page in the Pages folder. Name the page whatever you want, in my case *Page2.razor*.

### @Page
Page2.razor
```
@page "/2"
<h1>Vítejte na naší druhé stránce!</h1>
```
In our *MainLayout.razor* we set the main page link to / and now we can click through to *Home.razor* using our navbar.
```
<a href="/">Hlavní stránka</a>
```
Now you can see that even *Page2.razor* has our Layout.
With the **@page** directive, we set this page to be available at "https://<domain>/2". When the user visits this address, the HTML content following the **@page** directive will be displayed.

In our *MainLayout.razor* we set the link to /2 and now we can click through to *Page2.razor* using our navbar.

```
<a href="/2">Přejit na druhou stránku</a>
```

*MainLayout.razor*
```
@inherits LayoutComponentBase
<nav>
    <ul>
        <li><a href="/">Hlavní stránka</a></li>
        <li><a href="/2">Druhá stránka</a></li>
    </ul>
    <hr>
</nav>

<main> 
    @Body 
</main>
```

### [Parameter]
Attribute *[Parameter]* indicates that this parameter can be set by the parent component, which means that it can be used to pass data inside this component. We'll show how in a moment.

Let's create a folder /Pages/Components and in it the Rectangle.razor component.

*Rectangle.razor*:
```
<div class="rectangle" style="background-color: @Color;">

</div>

@code{
    [Parameter]
    public string? Color {get; set;}
}
```
Create a *Color* variable for the component and add it as a background color to the HTML element div ```style="background-color: @Color;"```.

Now in our main *Home.razor* component, we can create rectangles and change their colors as follows:
*Index.razor*:
```
@page "/"

<h1>Hello, World!</h1>

<div class="rectangles">
    <Rectangle Color="blue"/>
    <Rectangle Color="red"/>
    <Rectangle Color="yellow"/>
    <Rectangle Color="pink"/>
    <Rectangle Color="#FE8760"/>
</div>
```
Navbar.razor.css
```
ul{
    list-style-type: none;
    display: flex;
}

li{
    margin: 10px;
    font-weight: 600;
    color: blue;
}
```
Rectangle.razor.css
```
.rectangle {
    margin: 10px;
    width: 200px;
    height: 100px;
}
```
Home.razor.css
```
.rectangles{
    margin: 20px;
    display: flex;
}
```
## Using parameter in @page
<details>
<summary>Example</summary>

```
@page "/user/{Id:int}"

@if (Id == 1) {
    <p>První uživatel!</p> 
} else {
    <p>Ostatní uživatelé</p>
}
    
@code {
    [Parameter]
    public int Id { get; set; }
}
```

</details>

## Task 2
On the Home.razor page, create the *<Rectangle />* component and select its *Color* property from the input on the same page.
##

<details>
<summary>Solution:</summary>
Variable declaration:
	
```
string? c;
```

Add @bind to the input with our variable

```
<input @bind="c">

```
Add a variable to the Color property

```
<Rectangle Color="@c" />

```
</details>

## Dependency injection in Blazor
Simply put, dependency injection is a technique that allows components to get their dependencies from outside instead of creating them themselves.

For a better understanding, we will demonstrate DI in practice.

details>
<summary>First we define the *IMyService* interface, which will implement the method that returns the string.</summary>
	
```
interface IMyService {
    string WhatIsUjepReallyLike();
}
```

</details>

<details>
<summary>Next, we will define the *MyService* class that will implement the interface</summary>

```
public class MyService: IMyService
{
    public string? adjective;
    Random random = new Random();
    public string WhatIsUjepReallyLike(){
        string[] adjectives = {"super", "hrozný", "nejlepší", "nic extra"};
        int randomIndex = random.Next(0, adjectives.Length);
        string randomAdjective = adjectives[randomIndex];
        adjective = randomAdjective;
        return adjective;
    }
}
```
 
</details>

Now it's time to create our service in the *Program.cs* file.

Service lifetime:
- Singleton: Only one instance of the service is created for the lifetime of the application.
The other lifetimes are:
- Scoped
- Transient

We add our service to the code where we see builder services:
```
builder.Services.AddSingleton<MyService>();
```
<details>
<summary>Here is our component</summary>	

```
@page "/ujep"
@inject MyService service

<h3>Jaký je UJEP?</h3>
<button @onclick="GetUjepOnClick">Zjistit jaký UJEP doopravdy je!</button>

@if (service.adjective == null) {
    <p>Zatím nevím, jaký je UJEP</p>
} else {
    <p>UJEP je @service.adjective</p>
}

@code {
    public void GetUjepOnClick(){
        service.WhatIsUjepReallyLike();
    }
}
```

</details>


Now we can put the link to our component in navbar as well.
```
<li><a href="/ujep">What is UJEP really like</a></li>
```

## Part two
A Kanban board is a visual tool used to manage work tasks and their progress. It typically consists of columns that represent different stages of a process (e.g. "To Do", "In Progress", "Done"). Tasks are represented by tabs that move between columns according to their status. Kanban boards help the team see workflow, identify bottlenecks, and improve efficiency.

[Examples of a Kanban Board](https://www.google.com/search?sca_esv=697ef796fdf142b1&sca_upv=1&q=kanban+board&uds=ADvngMgqPfd500FffDLSjDoas1rZnlaZW-_XjFgB4fsGr6q-PMItPsU3TiACTUnY5dWkMFGt1WbAuBxpdYekSjAKCgDyGhv2zwcJKb9rvKSZnm5e_xJd7dWTaSxaDCOWJ8OZ_1NHOS1J62qJ8Oyt9osJh0TzHzLKeomT1KG3Z7qyE6BQGsRQbht5yibfcMe9JAdDicMf6noNtbpFCsBOCYCxx9TPqSvzm340OmLh5HFKUWHL9wzTBW7tJpPcOKuKcHALGPnbx-ItEyUJUbnIE5ERQSpikf3lOQ

## Part 1 - Models and fake databases
1. Create a Data folder in the root of the project
2.  Create the KanbanCardModel class
    1. string Title
    2. string Description
    3. DateOnly Deadline
3.  Create KanbanColumnModel class
    1. int Id
    2. string Title
    3. string BoxColor
    4. List\<KanbanCardModel> Cards
    5. Constructor that will auto increment the Id
4.  Create KanbanService class
    1. Contains List\<KanbanColumnModel>
5. We add our service in Program.cs

<details>

<summary> Checkpoint 1</summary>
KanbanCardModel.cs

```
public class KanbanCardModel
{
	public string? Title { get; set; }
	public string? Description { get; set; }
	public DateOnly? Deadline { get; set; }
}
```
KanbanColumnModel.cs
```
public class KanbanColumnModel
{
	private static int IdCount = 1;
	public int Id { get; set; }
	public string? Title { get; set; }
	public string? BoxColor { get; set; }
	public List<KanbanCardModel> Cards { get; set; } = new List<KanbanCardModel>();

	public KanbanColumnModel(string title, string color) 
	{
		this.Id = IdCount++;
		Title = title;
		BoxColor = color;
	}
}
```
KanbanService.cs
```
public class KanbanService
{
    public List<KanbanColumnModel> kanbanColumns = new List<KanbanColumnModel>();
}
```
Program.cs
```
// Add services to the container.
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();
builder.Services.AddSingleton<KanbanService>();
```

</details>

## Part 2 - Stylish Beginnings

I prepared a css stylesheet to make our application not look like it's from the 90's.

<details>

<summary> CSS Stylesheet</summary>

```
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

/* NAVBAR */
nav.navbar {
    background-color: #333;
    padding: 10px 40px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 30px;
}

nav.navbar a {
    padding: 12px 20px;
    border-radius: 5px;
    background-color: #007bff;
    transition: background-color 0.3s;
    color: white;
    text-decoration: none;
    margin-right: 20px;
    font-size: 20px;
}

nav.navbar a:hover {
    background-color: #0056b3;
    text-decoration: underline;
}

nav.navbar a:active {
    transform: translateY(1px);
}

/* KANBAN */
.kanban-board {
    display: flex;
}

.kanban-column {
    flex-shrink: 0;
    width: 300px;
    min-width: 200px;
    padding: 10px;
    background-color: #f4f4f4;
    border-radius: 5px;
    margin: 0 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s, box-shadow 0.3s;
    text-align: center;
}

.kanban-card {
    background-color: #fff;
    padding: 10px;
    margin: 10px 10px 10px 10px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.3s;
}

.kanban-column:hover {
    background-color: #f0f0f0;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.kanban-card:hover {
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* FORM */
.form-container {
    width: 400px;
    margin-left: 40px;
    margin-bottom: 15px;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.form-container h1{
    margin-bottom: 15px;
}

.form-container label {
    display: block;
    margin-bottom: 5px;
    font-size: 20px;
}

.form-container input[type="text"],
.form-container input[type="date"],
.form-container input[type="color"] {
    width: 100%;
    margin-bottom: 15px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

.form-container input[type="color"] {
    padding: 3px;
    border-radius: 5px;
    height: 40px;
    appearance: none;
}

.form-container button,
.form-container button[type="submit"] {
    width: 100%;
    padding: 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.form-container button:hover,
.form-container button[type="submit"]:hover {
    background-color: #45a049;
}

/* BUTTON */
.circular-button {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background-color: white;
    color: black;
    border: none;
    cursor: pointer;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s, transform 0.3s;
}

.circular-button:hover {
    background-color: #c2c2c2;
}

.circular-button:active {
    transform: scale(0.95);
}

.form-container button.button-red {
    width: 100%;
    padding: 10px;
    background-color: #e50000;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.form-container button.button-red:hover {
    background-color: #b32121;
}

```

</details>

We'll add it all to app.css in the wwwroot folder

in App.razor we have the header of our html pages, here you can for example import Bootstrap if you want
### TASK - Create a Blazor component that will be used as a form to create KanbanColumnModels

1. use using directive to use classes from namespace (AppName).Data
1. inject service
2. give it a route using page directive
3. make it interactive
4. make a form that combines the input data into the promoted
5. use these variables in the KanbanColumnModel constructor
6. make a method that adds a new KanbanColumnModel to the database using our service
7. we call the method on pressing the button

if the form is in \<div> with the kanban-column class, it will be relatively nicer

### Navbar

in MainLayout.razor we make a navbar that will route to Home and to that form


```
@inherits LayoutComponentBase

<nav class="navbar">
    <a href="/">Home</a>
    <a href="createform">Form</a>
</nav>

<main>
    @Body
</main>

<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

v Home.razor vytvořte list KanbanColumnModelů a přiřadte mu list z KanbanServisu uvnitr teto metody
```
protected override void OnInitialized()
{
	yourList = kanbanSvc.kanbanColumns;
}
```

make a foreach loop that renders the data from the KanbanColumn
that loop is wrapped in a \<div> with the kanban-board class

<details>
<summary>2. checkpoint</summary>

CreateForm.razor
```
@page "/createform"
@rendermode InteractiveServer
@using Data
@inject KanbanService KanbanSvc

<div class="form-container">
	<form>
		<h1>Create Column</h1>
		<label for="Title">Title</label>
		<input id="Title" type="text" @bind="newTitle" />
		<label for="Colour">Colour</label>
		<input id="Colour"type="color" @bind="color" />
		<button @onclick="AddColumn">Add todo</button>
	</form>
</div>


@code {
	public string newTitle;
	public string color = "#f4f4f4";

	public void AddColumn()
	{
		KanbanSvc.kanbanColumns.Add(new KanbanColumnModel(newTitle, color));
		newTitle = "";
		color = "#f4f4f4";
	}
}
```
Home.razor
```
@page "/"
@using Data
@inject KanbanService KanbanSvc
@rendermode InteractiveServer

<div class="kanban-board">
	@foreach (KanbanColumnModel column in Columns)
	{
		column.Title
		column.BoxColor
	}
</div>

@code {

	public List<KanbanColumnModel>? Columns { get; set; }
    protected override void OnInitialized()
    {
    	Columns = kanbanSvc.kanbanColumns;
    }
}
```

</details>

## 3. Part - Components and Components

for now it only renders text for us, we'll make a component to render a colored cube with a name

1. Create a KanbanColumn.razor component
2. obtains the KanbanColumnModel object using the Parameter attribute
3. write \<div> with class kanban-column
4. put a name in it and color the div using the object from the parameter

Modify Home.razor to create our new component in a foreach loop and insert KanbanColumnModel objects into the parameter


### Do the same but for Card


1. Create the KanbanCard.razor component
2. obtains the KanbanCardModel object using the Parameter attribute
3. write a \<div> with the kanban-card class
4. put object properties in it

Edit KanbanColumn.razor to create our new component in the foreach loop and insert KanbanCardModel objects into the parameter

<details>
<summary>3. Checkpoint</summary>

Home.razor
```
@page "/"
@using Data
@inject KanbanService KanbanSvc
@rendermode InteractiveServer

<div class="kanban-board">
	@foreach (KanbanColumnModel column in Columns)
	{
        <KanbanColumn Column="column"></KanbanColumn>
	}
</div>

@code {

	public List<KanbanColumnModel>? Columns { get; set; }
    protected override void OnInitialized()
    {
    	Columns = kanbanSvc.kanbanColumns;
    }
}
```
KanbanColumn.razor
```
@using Data
@rendermode InteractiveServer

<div class="kanban-column" style="background-color: @Column.BoxColor">
	<h2>@Column.Title</h2>
		@foreach (var card in Column.Cards)
		{
			<KanbanCard Card="card/>
		}
</div>

@code {
	[Parameter]
	public KanbanColumnModel? Column { get; set; }
}
```
KanbanCard.razor
```
@using Data
@rendermode InteractiveServer

<div class="kanban-card">
	<b>
		@Card.Title
	</b>
	<p>
		@Card.Description
	</p>
	<i>
		@Card.Deadline
	</i>
</div>

@code {
	[Parameter]
	public KanbanCardModel? Card { get; set; }
}
```
</details>

## Part 4 - Final Boss (phase 1/30)

- Create a button in KanbanColumn.razor that will redirect you to a new page EditForm.razor
- here create forms to create KanbanCardModels, edit KanbanColumnModel and delete it
- so that the page knows which KanbanColumnModel to put it in, we use the route parameter, which takes the Id

To redirect the button we use the NavigationManager service and its NavigateTo($"/editform/{Column.Id}") function - this sends us to /editform and puts Column.Id as a parameter

<details>
<summary>4. Checkpoint</summary>

EditForm.razor
```
@page "/editform/{id:int}"
@using Data
@inject KanbanService KanbanSvc
@inject NavigationManager NavMan
@rendermode InteractiveServer

@if (Column != null)
{
<div class="form-container">
	<h1>Add Card</h1>
	<form>
		<label for="Title">Title</label>
		<input id="Title" type="text" @bind="newTitle"/>
		<label for="Description">Description</label>
		<input id="Description" type="text" @bind="newDescription"/>
		<input type="date" @bind="newDeadline"/>
		<button @onclick="AddItem">Add Item</button>
	</form>
</div>

<div class="form-container">
	<h1>Edit Column</h1>
	<form>
		<label for"Title">Title</label>
		<input id="Title" type="text" @bind="Column.Title" />
		<label for"Color">Colour</label>
		<input id="Color" type="color" @bind="Column.BoxColor"/>
	</form>
</div>
<div class="form-container">
	<h1>Delete Column</h1>
	<button class="button-red" @onclick="DeleteColumn">DELETE</button>
</div>
}
else{
	<p>nothing here</p>
}


@code {
	[Parameter]
	public int Id { get; set; }
	public KanbanColumnModel? Column { get; set; }

	private string? newTitle;
	private string? newDescription;
	private DateOnly? newDeadline;

	protected override void OnInitialized()
	{
		Column = KanbanSvc.kanbanColumns.Find(x => x.Id == Id);
	}
	private void AddItem()
	{
		Column.Cards.Add(new KanbanCardModel() {Title = newTitle, Description = newDescription, Deadline = newDeadline });
		newTitle = "";
		newDescription = "";
	}
	private void DeleteColumn()
	{
		KanbanSvc.kanbanColumns.Remove(Column);
		NavMan.NavigateTo($"/");
	}
}

```

</details>

## Part 5 - Sidequests

Streamrendering - renders the html page before getting all the necessary data (from the database, API), after getting it rerendered
EditForm - built-in component for form creation with validation
