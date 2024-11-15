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

Az *XML* nyelv adatok **szöveges, platformfüggetlen reprezentációját** valósítja meg. Az XLM [[félig strukturált adatok]]at reprezentál. Az XML Előnye, hogy emberi szemmel és programmal is könnyen olvasható. Célja az egyszerű, általános használat. Eredetileg dokumentum leírásnak készült, sok más helyen is használják. Lekérdezésére használhatjuk az [[XPath]] nyelvet.

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

## XML tárolása relációs adatbázisokban

A Microsoft SQL, Oracle, PostgreSql mind XML-képes relációs adatbázisok. Relációs adatok mellett xml adat is szerepelhet bennük. A relációs a főadat, hiszen abból van több, ehhez köthető az XML adat.

Ilyenkor az oszlop adattípusa xml lesz, jól formázottnak kell lennie. Csatolható hozzá séma, amire automatikusan ellenőrzi a megfelelést. Kereshető, lekérdezhető, manipulálható, és index is definiálható rá.

### Index XML típusú oszlopra

Csak akkor kell, ha az XML adaton belül keresünk, az egész lekérdezéséhez nem használ indexet.

Két fajta index:
- elsődleges: teljes tartalmat indexeli
	- egy darab ilyen index definiálható
	- ha indexelt az oszlop, egy ilyen indexnek léteznie kell
		- `CREATE PRIMARY XML INDEX idxname on Table(Col)`
- másodlagos: konkrét xml elemre definiált
	- tetszőleges darabszámú definiálható
	- tovább segíti az optimalizációt
		- `CREATE XML INDEX idxname2 ON Table(Col)`
		- `USING XML INDEX idxname FOR VALUE`

### Séma hozzárendelése XML oszlophoz

Az adat validációját automatikusan elvégzi a rendszer a séma szerint, mint egy tartományi integritási kritérium. Lekérdezések optimalizálásához is használja. A hozzárendelés opcionális, nem kötelező.


### Lekérdezés

```sql
select Description.query('/product/num_of_packages')
	from Product <num_of_packages>1</num_of_packages>

select Description.value('(/product/num_of_packages)[1]','int') from Product 1)

select Name from Product
where Description.exist( '/product/num_of_packages eq 2')=1

```

### Manipulálás 

```sql
update Product
set Description.modify(
'replace value of
(/product/num_of_packages/text())[1] with "2"')
where ID=8

update Product
set Description.modify( 'insert 1 after (/product)[1]')
where ID=8

update Product set Description.modify('delete /product/a')
where ID=8
```

### FOR XML

Lekérdezés eredményének konvertálása XML formába:

```sql
select ID, Name from Customer
for xml auto
```

