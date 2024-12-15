---
aliases:
  - DDD
tags:
  - adatvez
  - datadriven
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-15 17:35
---

# Domain Driven Design

A Domain Driven Design a klasszikus [[Adatvezérelt rendszerek architektúrái|háromrétegű architektúra]] alternatívája.

>[!question]+ MI a baj a háromrétegű architektúrával?
>:LiSmile: Minimális overhead, kevés osztály, interfész
>:LiSmile: Hatékony: az adatbázis lekérdezést közvetlenül az adatigény formálja
>:LiSmile:/:LiFrown: A logika gyakran egy rétegben van
>:LiFrown: A [[Üzleti Logika Réteg|BLL]] függ az adat tárolási módjától 
>	- relációs / [[NoSQL]]
>	- Adatelérés megvalósítása [[Entity Framework]] / nHibernate / ...
>:LiFrown: A [[Üzleti Logika Réteg|BLL]] nem elválasztható a [[Adatelérési réteg|DAL-tól]], DB-től.


## Hagyma architektúra

Fordítsuk ki a klasszikus három rétegű architektúrát!
A belső mag független mindentől, nem tud semmit a külső héjakról
A külső héjak függetlenek a belsőktől

Az interfészeket belül definiáljuk és a külső héjak implementálják.

![[Pasted image 20241215182511.png]]

Így közvetlenebb lehet a kapcsolat a kód és az üzlet között. A kódban használjuk az eredeti üzleti elnevezéseket, hogy ne kelljen "fordítani" a fejlesztő és üzleti szakértő között. Ez segíti a kommunikációt, és a fejlesztés egyértelműségét.

### Domain model

Egy osztály, ami az üzleti modell adatait és viselkedését tartalmazza. Gyakran érték objektumokból (value object) épül fel.

### Value Object

A *Value Object* egy értéktípus, aminek nincs önálló identitása, tipikusan ID-ja sem az adatbázisban.
Nincs saját viselkedése, csak adatot tárol. Ezek az adatok gyakran összefüggenek, együtt érvényesek.

Létrehozás után nem változik.
Pl:
- Cím adatok
- Helyrajzi adatok
- Ár

## Domain layer = üzleti réteg

A legbelsőbb réteg, ami nem hivatkozik senkire, önmagában egész.
A domain modellek és szolgáltatások, a teljes üzleti logika itt van.
Nincs I/O művelet, csak logika
Könnyen tesztelhető, mert csak osztályokkal dolgozunk.

Nem tud semmit a külső rétegekről
- Milyen DB-t használunk?
- Milyen API-k vannak kifelé ajánlva?
- Hogyan vannak meghívva a külső szolgáltatások?

### Szolgáltatások

Nem minden funkció kapcsolódik egyértelműen egyetlen osztályhoz.
A funkció külön szolgáltatásba kerül: *Domain Sevice*
- Nem csak egy domain modellhez csatlakozik

Cél: a logikát a Domain modelbe tegyük, csak akkor használjunk szolgáltatást, ha arra feltétlen szükség van. 

## Application layer

A második réteg, elérhetővé teszi az üzleti logikát.
- használja a *domain layer*-t és más szolgáltatásokat, interfészeken keresztül
Alkalmazáslogikát valósít meg
- szemben az üzleti logikával
- nagyobb folyamatokat, pl. felhasználói eseteket implementál
- az üzleti logika akkor is érvényes, ha nincsenek számítógépek és kézzel intézünk mindent, az alkalmazás logika nem.
Itt sincs I/O művelet.


### Alkalmazás szolgáltatások

Ezek a szolgáltatások egy használati esetet implementálnak
- használhat más szolgáltatásokat interfészen keresztül
- használhatja a *domain model* osztályait

Koordinálja az egyéb szolgáltatásokat
- pl. meghívhatja az adatbázis olvasó/író funkciókat interfészen keresztül, elküld egy emailt, stb
- eseményeket publikál és azokra reagál
- független a külső szolgáltatások megvalósításától

DTO: a rétegek között közlekedő adatosztályok
- Nem egyeznek meg a domain osztályokkal, interfész specifikusak
- Javaslat: **ne osszuk meg őket különböző használati esetek közt**

## Persistence layer

A legkülsőbb réteg, ahol megvalósul az összes I/O művelet. Itt nem lehet üzleti / folyamatlogika. Tud mindent a belső rétegekről, kiszolgálja az *application layer*-t, implementálva a szükséges interfészt.

Repositoryk - adattárolás.


### Repository

Egy domainmodell gyűjteményt reprezentál, feladata a domain modellek perzisztálása adatbázisba, fájlrendszerbe, stb.
A tárolás módja az alkalmazás szempontjából részletkérdés.

Domain aggregétenként külön repository-t érdemes létrehozni
- Pl vásárló, számla, termék, stb.

A DbContext-et önmagában tekinthetjük egy Repository-nak.
- de így nehezen korlátozzuk az interfészét, az EF beszivárog a Domain-be.
- de külön megírva sokkal több a "triviális" kód.

>[!success]+ Előnyök
>- Az üzleti logika lazán kapcsolódik az adattárolás módjához
>- Az adattár/elérési módszer cserélhető a logika megváltoztatása nélkül
>- Egyértelműen szétválnak az üzleti és az adatelérési funkciók.

>[!error]+ Hátrányok
>- Implementációs overhead
>	- Minden adatelérési feladat külön funkcióként jelenik meg
>- Az üzleti logika két részben van: lekérdezések logikája a repository-ban.
>- A megoldás teljesítménye lehet szuboptimális, **külön figyelmet igényel**

### Aggregate Roots
 
 Üzleti entitásokat kombinál össze, amelyek ugyanazokban a felhasználói esetekben vesznek részt. Ezek a csoportok rendelkeznek viselkedéssel és biztosítják a konzisztenciát.

Egy művelet ideális esetben egyetlen aggregate-et érint, ami egyetlen tranzakció.

**Csak aggregate root-okra tarthatunk referenciákat!**

A benne levő entitásokhoz nem lehet kívülről hozzáférni
- Azoknak nincs értelme, mert minden művelet csak a szülő kontextusában értelmezhető
- A konkurenciakezelés az aggregate rootban kerül megvalósításra
	- minden gyerek entitás módosítása a gyökérelemen keresztül történik

Például: Order (root) -> OrderItem, külön nem férünk hozzá OrderItemekhez!

Minden aggregate-nek saját repositoryja van, elfedve a tárolási részleteket.

Egy aggregate hivatkozhat más aggregate-ekre, DE ez nem jelenti, hogy **egy konzisztancia határon** belül lennének: mindkettő módosítása saját tranzakcióban történik.
- A hivatkozás tipikusan ID-kon keresztül történik
- Módosítani csak az egyik aggregate root-on keresztül lehet.

## Infrastructure Layer

A legkülsőbb réteg, ami mindent tud a belső rétegekről. 
Ide tartoznak a:
- külső API hívások, szolgáltatások elérése
- eseményfigyelők
- naplózás
- cache-elés
- konfiguráció
- stb.

## Presentation Layer

A legkülsőbb réteg, ami mindent tud a belső rétegekről. 
A szolgáltatást elérhetővé teszi a felhasználók és más rendszerek számára.
- [[REST API]]
- UI

## Interfészek és DI

Az interfészeket a belső rétegek definiálják, és a külső rétegek valósítják meg.
A belső réteg egy interfészimplementációt kap, amit meghív:
- az alkalmazás futtató host állítja össze, hogy melyik interfész milyen implementációt kap: függőségek injektálása
- pl teszt rendszerben máshogy tároljuk az adatokat, külső apikat mockoljuk, stb.


## DDD vs Klasszikus

Domain Driven Design:
- Az üzleti modell vezérel
- szétválasztott felelősségek (Repo)
- könnyebben tesztelhető (DI)
- üzleti logika nélkül túl nagy overhead
-  komplex megvalósítás, sok tanulás
- teljesítmény kockázat
- függőségek: 
	- BLL <- App <- Infra (DAL) -> DB

Klasszikus:
- a tárolás struktúrája, módja vezérel
- a felelősségek keveredhetnek
- a tesztelés nagyobb kihívás lehet
- egyszerű esetben kis overhead
- egyszerűbb
- naiv implementáció jó teljesítményt ad
- függőségek
	- API -> BLL -> DAL -> DB

## Tipikus hibák

- Domain modellek saját viselkedés nélkül
	- minden logika a domain szolgáltatásokban van, és nem a modellben
	-  ha sehol nincs logika -> **ne használj DDD-t!**
- Kezdjük az adatbázisnál...
	- A belső rétegekkel, a domain modellel kezdjük
	- A repository használja a domain modelleket, és az Application layer interfészét valósítja meg -> nehéz innen indulni

## Függőségek

- a domain nem függ senkitől, csak domain osztályok, semmi technológia
- application hivatkozik a domainre
	- tranzakció határok itt, de nem ismeri a tárolási technikát
	- aggregate rootok kezelése, összefésülése
	- CQRS pattern
- infrastructure: hivatkozik a domainre és az applicationre
	- összevonható a repositoryval
	- Repository megvalósítás