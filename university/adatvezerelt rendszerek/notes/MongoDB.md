---
aliases: 
tags:
  - adatvez
  - 5semester
  - uni
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-13 19:54
---

# MongoDB


A MongoDB egy [[NoSQL]] adatbázis, melynek segítségével dokumentum alapú adattárolást valósíthatunk meg. Sémája dinamikus, rugalmas, ellentétben a relációs adatbázisokkal.

## Felépítése

>[!note]+ Rendszer architektúra
>
> ![[Pasted image 20241113200901.png]]


**Logikai felépítés**:
- klaszer
	- szerver
		- [[MongoDB#Adatbázis|adatbázis]]
			- [[MongoDB#Gyűjtemény|gyűjtemény]]
				- [[MongoDB#Dokumentum|dokumentum]]

### Dokumentum

A dokumentum egy [[JSON]] / BSON, ami *kulcs-érték párokat tartalmaz*. Az érték lehet szöveg, szám, bool, null, tömb, vagy beágyazott objektum. A *kulcs alapértelmezetten az \_id implicit mező*, de ez lecserélhető.
Mérete maximálisan 16MB.

### Gyűjtemény

A gyűjtemény a relációs adatbázisok tábláinak analógiája, de nem rendelkezik sémával, így nem kell létrehozni, definiálni. Lényegében *hasonló dokumentumok* *gyűjtőhelye*, melyen indexeket definiálhatunk. Mivel nincs séma, nincs *tartományi integritási kritérium*.

> [!example]- Példa
> ```json
> {
> 	name: "Sue",
> 	age: 26
> }
> {
> 	name: "John",
> 	age: 32,
> 	title: "CEO"
> }
> {
> 	name: "Ronald McDonald",
> 	age: "born in 1955"
> }
> ```

### Adatbázis

Az adatbázis ugyanazt a célt szolgálja, mint a relációs adatbázisoknál. Az alkalmazás *adatainak logikai összefogása*. A hozzáférési jogosultságok is adatbázis szinten adhatók. Neve case sensitive, tipikusan lowercase szokás használni.

>[!warning]+ Relációs Séma / [[MongoDB]]
> ![[Pasted image 20241113201712.png]]

### Alapértelmezett kulcs

Az alapértelmezett kulcs minden dokumentumban az *\_id*, ezen kívül nincs kulcsmező, és *nem is lehet definiálni*. Lehetőség van azonban *indexet készíteni*. Az *ObjectId*-t a kliens driver vagy a szerver generálja. Ez egy 12 bájtos ID, melyből 4 bájt az **időbélyeg**, 5 bájt **random**, 3 bájt pedig **számláló**, amik együtt egy globálisan egyedi azonosítót képeznek. Összetett kulcs nincs, csak összetett index.

### Beágyazás, tömbök

>[!note]+ Hivatkozás más dokumentumra
> ![[Pasted image 20241113203551.png]]
>
> ![[Pasted image 20241113203612.png]]

Join helyett normalizálást használunk.


- 1-1 kapcsolat -> beágyazás
- 1-több kapcsolat -> beágyazás tömbként, vagy hivatkozás


## MongoDB sématervezés

A séma tervezésénél általában a beágyazásokat preferáljuk a hivatkozások helyett, de a *dokumentum maximum mérlete* *korlátoz* ebben.


## Tranzakciók

A MongoDB-ben egy [[NoSQL#Tranzakciók|tranzakció]] frissítése atomi. Támogatja az [[NoSQL#Elosztott ACID tranzakciók|elosztott tranzakciókat]], akár ACID tulajdonságokkal is, de a teljesítményt ez jelentősen rontja.

## MongoDB protokollja

Wire protocol: TCP/IP alapú bináris
Kliens alkalmazások, driverek népszerú platformokhoz.
A kérés és a válasz is egy [[JSON]] dokumentum. Ez a MongoDB "[[MongoDB programozása#MongoDB "nyers" nyelve|nyers nyelve]]". Fejlettebb nyelveken, pl [[C#]] -ban [[MongoDB programozása#MongoDB .NET|nyelvhez illeszkedő driver]] van.

