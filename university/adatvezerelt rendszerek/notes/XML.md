---
aliases: 
tags:
  - cs
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-04 13:05
---

# Extensible Markup Language (XML)

Az *XML* nyelv adatok **szöveges, platformfüggetlen reprezentációját** valósítja meg. Előnye, hogy emberi szemmel és programmal is könnyen olvasható. Célja az egyszerű, általános használat. Eredetileg dokumentum leírásnak készült, sok más helyen is használják.

>[!example]+ XML példa
>Osztályok:
>
>```csharp
>public class Customer
>{
>	public string Name;
>	public DateTime Registered;
>	public Address Address
>}
>
>public class Address
>{
>	public string City;
>	public int Zipcode;
>}
>```
>XML:
>
> ![[Pasted image 20241104131552.png]]

Az XML fájlok XML [[encoding]] ban mentődnek.

## XML névterek

Mivel a tagnevek szabadon választhatóak, előfordulhat ütközés. Ennek kiküszöbölésére szolgálnak az *XML névterek*, hasonlóan a [[C++]] / [[C#]] névterekhez és a [[Java]] package-khez. A névtér egy prefix: <*ns*:tag>. Az XML fájl elején deklarálnunk kell őket: `xmlns:ns="URI"`.

## CML
