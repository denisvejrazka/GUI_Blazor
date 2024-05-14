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
