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
