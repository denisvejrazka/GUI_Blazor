# Blazor tutoriál

##### Tutoriál je rozdělen do dvou částí. V první části se seznámíme se základními koncepty frameworku Blazor a druhá část bude věnována vývoji kanban tabule.
#

Blazor je open-source framework pro tvorbu webových aplikací od Microsoftu. Umožňuje vývojářům budovat interaktivní uživatelská rozhraní s využitím jazyků C# a HTML.

## Proč používat Blazor?
- Známý jazyk: Blazor používá C#, takže vývojáři s touto platformou nebudou mít problém se zorientovat.

- Komponenty pro opakované použití: Blazor podporuje tvorbu opakovaně použitelných komponent, čímž se zjednodušuje a zefektivňuje vývoj.

- Server-side i client-side: Blazor umožňuje vývoj aplikací jak na straně serveru (Blazor Server), tak na straně klienta (Blazor WebAssembly).

- Široká škála nástrojů a knihoven: Pro Blazor je k dispozici bohatá paleta nástrojů a knihoven, které usnadňují vývoj a rozšiřují jeho funkčnost.

- Interoperabilita s JS: Aplikace Blazor může volat funkce JavaScriptu z metod .NET a metody .NET z funkcí JavaScriptu. Tyto scénáře se označují jako interoperabilita JavaScriptu (zprostředkovatel komunikace JavaScript).

## Modely hostování Blazor aplikací
- Blazor WebAssembly - tento model hostování umožňuje spouštění Blazor aplikací přímo v prohlížeči. Klientská část aplikace (kód napsaný v C#) je kompilována do WebAssembly, což je binární formát, který může být spuštěn v moderních webových prohlížečích.

- Blazor Server - v tomto modelu je aplikace hostována na serveru a komunikuje s klientem pomocí protokolu SignalR, což je technologie pro real-time komunikaci mezi klientem a serverem. Klientská část aplikace (HTML, CSS, JavaScript) je vykreslována na straně serveru a posílána do prohlížeče jako statická stránka. Klient poté komunikuje s serverem pomocí SignalR pro interaktivní aktualizace a události.

- Blazor Hybrid - umožňuje kombinovat desktopové a mobilní nativní klientské architektury s .NET a Blazor.

Více o modelech hostování zde: https://learn.microsoft.com/cs-cz/aspnet/core/blazor/hosting-models?view=aspnetcore-8.0


## První část

## Jak začít s Blazorem?
- Doporučujeme mít nainstalované Visual Studio, ale lze vyvíjet například i ve Visual Studio Code, pokud máte třeba C# Dev Kit. Nicméně tento tutoriál bude zaměřen primárně na Visual Studio.
- Pokud nemáte nainstalovaný Blazor, otevřete Visual Studio Installer a v něm vyberte následující položku:


![asp net a web](https://github.com/denisvejrazka/GUI_Blazor/assets/112482063/2bb0b434-9f1e-4e54-9af4-2e25f81ba405)

## Komponenty
Aplikace Blazor jsou založené na komponentách. Komponenta v Blazor je prvek uživatelského rozhraní, například stránka, dialogové okno nebo formulář pro zadávání dat. 

Komponenty jsou obvykle psány ve formě stránky jazyka Razor s příponou souboru *.razor*.

### Vytvoření Komponenty
Pro vytvoření komponenty klikneme pravým tlačítkem na složku Components a vybereme *Přidat* a dále "Komponenta Razor".

## Základní syntaxe

Při vytvoření komponenty se můžeme vrhnout do základní syntaxe.

### Použití proměnných v HTML kódu
```
<h3>Moje jméno je @Name</h3>

@code {
	string Name = "Honza";
}

```

### Podmínky
```
@if (GenerateRandomNumber() == 2)
{
	<h3>Vygenerované číslo je 2</h3>
}
else
{
	<h3>Vygenerované číslo není 2</h3>
} 

@code {
    private int randomNumber;
    private Random random = new Random();

    private int GenerateRandomNumber() => random.Next(1, 3); 
}
```

### Cykly
```
@for (int i = 1; i <= 5; i++)
{
	<p style="color: green;" >Číslo @i</p>
}
```

## Counter
Do složky Components si vytvoříme *Counter.razor*

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

Důležitý zde je **@onclick** event handler, který nám umožňuje spustit funkci IncrementCount() na kliknutí tlačítka.

## Úkol 1
Předělejte kód komponenty *Counter.razor*, aby číslo zezelenalo pokud je dělitelné 2 (bez zbytku).

<details>
<summary>Řešení úkolu 1</summary>

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

#### Stylování pomocí HTML atributu style, není optimální, tudíž jedna z dalších možností je, pravým tlačítkem klepnout na komponentu, přidat položku *"jméno komponenty.razor"* a přidat příponu souboru *.css*, nyní máte k této komponentě zvláště *.css* soubor.

### Binding
- One-way Binding: Jednosměrný binding umožňuje zobrazit data z C# do HTML. Pokud se data v C# změní, tak se změní i zobrazení v HTML. Jednosměrný binding se používá ve směru od dat k uživatelskému rozhraní.
```
<p>Vaše jméno: @jmeno</p>

@code {
    string jmeno = "Honza";
}
```

- Two-way Binding: Dvousměrný binding umožňuje zobrazit data z C# do HTML a zároveň umožňuje aktualizovat data v C# na základě změn v HTML. To znamená, že pokud uživatel změní hodnotu v HTML, tak se tato změna propaguje zpět do kódu C# a data jsou aktualizována. Pro dvousměrný binding se používá direktiva **@bind**.
```
<input type="text" @bind="jmeno" />

<p>Vaše jméno: @jmeno</p>

@code {
    string jmeno = "Honza";
}
```
Dvousměrný binding se používá pro interaktivní prvky, jako jsou formuláře.

<details>
<summary>Praktický příklad</summary>
	
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

### Layout
Layout je komponenta Razor , která sdílí značky s komponentami, které na ni odkazují. Může to být například navbar a nebo footer. Zkrátka část webové stránky, která bude sdílena na mnoha odkazech webové stránky.

V ukázce kódu můžete vidět jednoduchý Navbar, který nás nikam nebude odkazovat, ale slouží pouze pro demonstraci Layoutu.

Komponenta dědí ze třídy *LayoutComponentBase*, což je základní volitelná třída pro komponenty, které představují Layouty.
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
Direktiva ***@Body*** označuje tu dynamickou část webu, která nezůstane stejná.

Css k Navbaru je následující:
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

Pokud teď půjdete na *Home.razor* bude tam Navbar.
Tímto ovšem ale neověříme, zda tam Navbar zůstane i na jiném odkazu webové stránky. Proto si vytvoříme libovolnou stránku ve složce Pages. Stránku pojmenujte jak chcete, v mém případe *Stranka2.razor*.

### @Page
Stranka2.razor
```
@page "/2"
<h1>Vítejte na naší druhé stránce!</h1>
```
V našem *MainLayout.razor* nastavíme odkaz hlavní stránky na / a nyní se můžeme prokliknout na *Home.razor* pomocí našeho navbaru.
```
<a href="/">Hlavní stránka</a>
```
Nyní vidíte, že i *Stranka2.razor* má náš Layout.
Direktivou **@page** nastavíme, že tato stránka bude dostupná na adrese "https://<doména>/2". Když uživatel navštíví tuto adresu, zobrazí se obsah HTML následující za direktivou **@page**.

V našem *MainLayout.razor* nastavíme odkaz na /2 a nyní se můžeme prokliknout na *Stranka2.razor* pomocí našeho navbaru.
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
Atribut *[Parameter]* označuje, že tento parametr může být nastaven ze strany rodičovského komponentu, což znamená, že může být použit k předávání dat dovnitř tohoto komponentu. Hned si ukážeme jak.

Vytvoříme si složku /Pages/Components a v ní komponentu Rectangle.razor.

*Rectangle.razor*:
```
<div class="rectangle" style="background-color: @Color;">

</div>

@code{
    [Parameter]
    public string? Color {get; set;}
}
```
Komponentě vytvoříme proměnnou *Color* a přidáme ji jako barvu pozadí do HTML elementu div ```style="background-color: @Color;"```.

Nyní v naší hlavní komponentě *Home.razor* si můžeme vytvářet obdélníky a měnit jim barvy následujícím způsobem:

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

## Použití parametru v @page
Parametr můžeme použít i s direktivou @page.
<details>
<summary>Praktický příklad</summary>

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

## Úkol 2
Na stránce Home.razor vytvořte komponentu *<Rectangle />* a její vlastnost *Color* vyberte z inputu na stejné stránce.
##

<details>
<summary>Řešení:</summary>
Deklarace proměnné:
	
```
string? c;
```

Přidáme do inputu @bind s naší proměnnou

```
<input @bind="c">

```
Přidáme proměnnou do vlastnosti Color

```
<Rectangle Color="@c" />

```
</details>
	
## Dependency injection v Blazoru
Zjednodušeně řečeno, dependency injection je technika, která umožňuje komponentám získávat své závislosti zvenčí, místo toho aby si je vytvářely samy.

Pro lepší pochopení si DI ukážeme v praxi.
<details>
<summary>Jako první si nadefinujeme rozhraní *IMyService*, které bude implementovat metodu, která vrací string.</summary>
	
```
interface IMyService {
    string WhatIsUjepReallyLike();
}
```

</details>

<details>
<summary>Dále si nadefinujeme třídu *MyService*, která bude implementovat rozhraní</summary>

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


Nyní je čas si vytvořit naší službu v souboru *Program.cs*.

Životnost služby:
- Singleton: Po celou dobu životnosti aplikace je vytvořena pouze jedna instance služby.
Další životnosti jsou:
- Scoped
- Transient

Ke kódu, kde vidíme builder services si přidáme naší službu:
```
builder.Services.AddSingleton<MyService>();
```
<details>
<summary>Zde je naše komponenta, která bude využivat službu</summary>	

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


Nyní si odkaz na naší komponentu můžeme vložit také do navbaru.
```
<li><a href="/ujep">Zjistit jaký je UJEP</a></li>
```

## Druhá část
# Praktická část - vytvoření Kanban tabule
Kanban tabule je vizuální nástroj používaný k řízení pracovních úkolů a jejich postupu. Typicky se skládá ze sloupců, které reprezentují různé fáze procesu (např. "To Do", "In Progress", "Done"). Úkoly jsou reprezentovány kartami, které se přesouvají mezi sloupci podle jejich stavu. Kanban tabule pomáhá týmu vidět pracovní tok, identifikovat překážky a zlepšovat efektivitu.

[Příklady kanban tabulí](https://www.google.com/search?sca_esv=697ef796fdf142b1&sca_upv=1&q=kanban+board&uds=ADvngMgqPfd500FffDLSjDoas1rZnlaZW-_XjFgB4fsGr6q-PMItPsU3TiACTUnY5dWkMFGt1WbAuBxpdYekSjAKCgDyGhv2zwcJKb9rvKSZnm5e_xJd7dWTaSxaDCOWJ8OZ_1NHOS1J62qJ8Oyt9osJh0TzHzLKeomT1KG3Z7qyE6BQGsRQbht5yibfcMe9JAdDicMf6noNtbpFCsBOCYCxx9TPqSvzm340OmLh5HFKUWHL9wzTBW7tJpPcOKuKcHALGPnbx-ItEyUJUbnIE5ERQSpikf3lOQ&udm=2&prmd=ivnbz&sa=X&ved=2ahUKEwjD75yJ4pCGAxXjUEEAHe22DnYQtKgLegQIChAB&biw=1920&bih=1067&dpr=1.5)

## 1. Část - Modely a fake databáze
1.  Vytvoříme složku Data v kořenu projektu
2.  Vytvoříme třídu KanbanCardModel
    1. string Title
    2. string Description
    3. DateOnly Deadline
3.  Vytvoříme třídu KanbanColumnModel
    1. int Id
    2. string Title
    3. string BoxColor
    4. List\<KanbanCardModel> Cards
    5. Konstruktor který bude auto inkrementovat Id
4.  Vytvoříme třídu KanbanService
    1. Obsahuje List\<KanbanColumnModel>
5. Přídáme náš servis v Program.cs

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

## 2. Část - Stylový začátky

Připravil jsem css stylesheet aby naše aplikace nevypadala jak z devadesátých let.

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


Celý to přidáme do app.css v wwwroot složce

v App.razor máme hlavičku našich html stránek, tady si mužete třeba naimportovat Bootstrap pokud chcete

### ÚKOL - Vytvořte Blazor komponentu, která bude sloužit jako formulář na vytváření KanbanColumnModelů

1. použijte using directive abychom mohli použít třídy z namespace (AppName).Data
1. injekujte servis
2. dejte mu route pomocí page directive
3. udělejte aby byl interaktivní
4. udělejte formulář, která nabinduje input data do promněných
5. typo promněnné použijeme v konstruktoru KanbanColumnModel
6. udělejte metodu, která přídá nový KanbanColumnModel do databáze pomoci našeho servisu
6. metodu zavoláme na stisknutí tlačítka

když ten formulář bude v \<div> s třídou kanban-column tak bude relativně hezkej 

### Navbar

v MainLayout.razor uděláme navbar, který bude routovat do Home a do toho formuláře

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

udělejte foreach cyklus, který vyrenderuje data z KanbanColumn
ten cyklus je obalen v \<div> s třídou kanban-board

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

## 3. Část - Komponenty a Komponenty

zatím nám to vykresluje jen text, uděláme komponentu na vykreslení barevného kvádru s názvem

1. Vytvoř blazor komponentu KanbanColumn.razor
2. pomocí atributu Parameter získá objekt KanbanColumnModel
3. napište \<div> s třídou kanban-column
4. do toho vložte název a obarvěte div pomocí objektu z parametru

Upravte Home.razor aby v foreach cyklu vytvářel naši novou komponentu a do parametru vložil KanbanColumnModel objekty


### Udělat to samé ale pro Card


1. Vytvoř blazor komponentu KanbanCard.razor
2. pomocí atributu Parameter získá objekt KanbanCardModel
3. napište \<div> s třídou kanban-card
4. do toho vložte vlastnosti objektu

Upravte KanbanColumn.razor aby v foreach cyklu vytvářel naši novou komponentu a do parametru vložil KanbanCardModel objekty


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

## 4. Část - Final Boss (phase 1/30)

- Vytvořte tlačítko v KanbanColumn.razor, který vás přesměruje do nové stránky EditForm.razor
- zde vytvořte formuláře na vytváření KanbanCardModelů, editování KanbanColumnModelu a jeho smazání
- aby stránka věděla do jakého KanbanColumnModelu to dát, využijeme route parametr, který vezme Id

Abychom tlačítkem mohli přesměrovat použijeme servis NavigationManager a jeho funkci NavigateTo($"/editform/{Column.Id}") - tohle nás pošle do /editform a jako parametr tam dal Column.Id

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

## 5. Část - Sidequests

Streamrendering - vyrenderuje html stránku dřív než získal všechny potřebná data (z databáze, API), po získání se rerenderuje
EditForm - vestavěná komponenta na vytváření formulářu s validací
