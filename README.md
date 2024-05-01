
# Blazor tutoriál

Blazor je open-source framework pro tvorbu webových aplikací od Microsoftu. Umožňuje vývojářům budovat interaktivní uživatelská rozhraní s využitím jazyků C# a HTML.

## Proč používat Blazor?
- Známý jazyk: Blazor používá C#, takže vývojáři s touto platformou nebudou mít problém se zorientovat.

- Komponenty pro opakované použití: Blazor podporuje tvorbu opakovaně použitelných komponent, čímž se zjednodušuje a zefektivňuje vývoj.

- Server-side i client-side: Blazor umožňuje vývoj aplikací jak na straně serveru (Blazor Server), tak na straně klienta (Blazor WebAssembly).

- Široká škála nástrojů a knihoven: Pro Blazor je k dispozici bohatá paleta nástrojů a knihoven, které usnadňují vývoj a rozšiřují jeho funkčnost.

- Interoperabilita s JS: Aplikace Blazor může volat funkce JavaScriptu (JS) z metod .NET a metody .NET z funkcí JS. Tyto scénáře se označují jako interoperabilita JavaScriptu (zprostředkovatel komunikace JS).

## Modely hostování Blazor aplikací
- Blazor WebAssembly - tento model hostování umožňuje spouštění Blazor aplikací přímo v prohlížeči. Klientská část aplikace (kód napsaný v C#) je kompilována do WebAssembly, což je binární formát, který může být spuštěn v moderních webových prohlížečích.

- Blazor Server - V tomto modelu je aplikace hostována na serveru a komunikuje s klientem pomocí protokolu SignalR, což je technologie pro real-time komunikaci mezi klientem a serverem. Klientská část aplikace (HTML, CSS, JavaScript) je vykreslována na straně serveru a posílána do prohlížeče jako statická stránka. Klient poté komunikuje s serverem pomocí SignalR pro interaktivní aktualizace a události.

- Blazor Hybrid - Blazor Hybrid je kombinací obou předchozích modelů hostování. Část aplikace běží na serveru a část běží ve webovém prohlížeči pomocí WebAssembly.

Více o modelech hostování zde: https://learn.microsoft.com/cs-cz/aspnet/core/blazor/hosting-models?view=aspnetcore-8.0

## Jak začít s Blazorem?
- Doporučujeme mít nainstalované Visual Studio, ale lze vyvíjet například i ve Visual Studio Code, pokud máte třeba C# Dev Kit. Nicméně tento tutoriál bude zaměřen primárně na Visual Studio.
- Pokud nemáte nainstalovaný Blazor, otevřete Visual Studio Installer a v něm vyberte následující položku:


![asp net a web](https://github.com/denisvejrazka/GUI_Blazor/assets/112482063/2bb0b434-9f1e-4e54-9af4-2e25f81ba405)




