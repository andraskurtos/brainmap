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

Az *XML* nyelv adatok **szöveges, platformfüggetlen reprezentációját** valósítja meg. Előnye, hogy emberi szemmel és programmal is könnyen olvasható. Célja az egyszerű, általános használat. Eredetileg dokumentum leírásnak készült, sok más helyen is használják. Lekérdezésére használhatjuk az [[XPath]] nyelvet.

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

## XML jellemzők

>[!success]+ Előnyök:
>- Szöveges adatreprezentáció:
>	- Platformfüggetlen
>	- Szabványos megoldások
>	- Dokumentált séma
>	- Könnyen értelmezhető
>- Típusos:
>	- Séma leírás
>	- Származás

>[!fail]+ Hátrányok:
>- Gráfreprezentáció nehézkes
>- Nem egyértelmű adatreprezentáció
>	- Attribútum?
>	- Gyerek elem?
>	- Null?
>- Szöveges --> nagyobb méret

## XML és .NET

Adatok szerializálhatóak a
*System.Xml.Serialization.XmlSerializer*-el:

```csharp
var ser = new XmlSerializer(typeof(C));
ser.Serialize(<stream>, <obj>);
myobj = (C)ser.Deserialize(<stream>);
```

A testreszabás attribútumokkal történik:

```csharp
[XmlElement("Cim")]
public class Address
{
	[XmlAttribute("Varos")]
	public string City;
	[XmlIgnore]
	public int SzamoltTavolsag;
}
```

## Séma

Egy XML dokumentum *jól formázott*, ha szintaktikailag megfelelő a tartalma, tehát *minden nyitótag be lett zárva*, zárójelezés szabályai szerint, *egyetlen gyökér eleme van*, stb.

A tartalom érvényesége azonban más kérdés. Kérdéses, jó néven vannak-e benne a tagek, olyan tartalom van-e azokban, amire számítunk.

Sémával leírható a várt tartalom, pl DTD, XSD --> validálás: egy adott XML dokumentum megfelel-e egy adott sémának.


## [[XML]] vs [[JSON]]

![[JSON#XML vs JSON]]

