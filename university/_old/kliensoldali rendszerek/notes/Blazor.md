---
aliases: 
tags:
  - uni
  - 5semester
  - client
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-19 19:19
---

# Blazor

A Blazor a Microsoft legújabb .NET alapú webalkalmazás keretrendszere.
Razor szintaxis, újrahasznosítható komponensek
Megosztható kód/logika szerver-kliens között.


## Projekt felépítése

Gyakran egyetlen projekt
**wwwroot**: a webkiszolgáló statikus tartalma
**Program.cs**:szerver oldal belépési pontja
**Components**: a komponensek kódja .razor-ben.
- App.razor: fő HTML kód
- Layoutok
- Pages

## Counter példa

HTML-szerű szintaktika
- együtt a kód és a megjelenés
- PageTitle -> az oldal címe

Kifejezés kiértékelés: @jel

```csharp
<PageTitle>Counter</PageTitle>
<h1>Counter</h1>
<p role="status"> Current count: @currentCount</p>
<button class="btn btn-primary"
	@onclick="IncrementCount">Click me</button>

@code {
	private int currentCount = 0;
	private void IncrementCount() => currentCount++;
}
```

A komponens felhasználása Home komponensben:

```csharp
<PageTitle>Home</PageTitle>
<h1>Hello, World! </h1>
Welcome to your new app.
<Counter/>
```

### Komponens paraméterrel

publikus property a \[Paramerter] attribútummal
Kívülről állítható a bemeneti paraméter.

```cs
@code {
	private int currentCount=0;
	[Parameter]
	public int IncrementAmount {get;set;}=1;
	private void Increment() => currentCount()+=IncrementAmount;
}
```

```cs
<Counter IncrementAmount="3" />
```

