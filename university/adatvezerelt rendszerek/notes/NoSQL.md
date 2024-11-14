---
aliases:
  - no-sql
  - nosql
tags:
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-13 19:55
---

# NoSQL

>[!summary]+ Motiváció:
>Relációs adatbázisok használatakor az alkalmazás fejlődésével a **séma egyre bonyolódik**, ezért nagyban nő az adatbázis mérete.
>Bizonyos méret/tranzakciószám után **nehezen kezelhető** az adatbázis, és költséges a **magas rendelkezésreállás biztosítása**.
>
>*Megoldás: Hagyjuk el a tradicionális, szigorú sémát --> NoSQL*

>[!warning]+ SQL vs NoSQL
>[[SQL]]:
>- relációk, kényszerek
>- előre definiált séma
>- vertikálisan, felfele skálázódik
>- tábla alapú
>- több soros műveletek
>- klasszikus ACID tranzakciók
>
>NoSQL:
>- nem-relációs, kevés kényszer
>- dinamikus, rugalmas, változó séma
>- horizontálisan, oldalra skálázódik
>- dokumentum alapú
>- dokumentum/[[JSON]] műveletek
>- eventual consistency

## [[Tranzakciókezelés|Tranzakciók]]

### Elosztott ACID tranzakciók

Az eloszott ACID tranzakciókban *több adatbázisszerver is részt vesz* egy tranzakcióban. Mindig van egy *tranzakció menedzser/leader*, aki vezérli a végrehajtást. ACID tulajdonásogokkal rendelkeznek, *rövid tranzakció és kommitidő esetén működnek jól*.

Szorosan csatolt rendszer, adatbázis szintű integrációval.
Zárak --> várakozás VAGY snapshot olvasás (pesszimista/optimista megközelítés)


### Hosszan futó tranzakciók

A hosszan futó tranzakciólnál *nincs* adatbázisszerű *commit/rollback*, az adatok konzisztenciájának visszaállítását *saját, kézi mechanizmus* biztosítja. Speciális adattárakkal
Elosztott heterogén megoldások


### Globálisan elosztott rendszerek

A globálisan elosztott rendszereknél (pl AWS, Azure stb.) hatalmas az adatmennyiség, nagy a terhelés, a szükséges terület. Az adatokról több másolat is létezik, egymástól messze.

>[!summary]+ **CAP-tétel**
>Az alábbi három képesség nem biztosítható egyszerre egy elosztott rendszerben, ami adatreplikációt használ:
>
>1. Konzisztencia: mintha **egyetlen** kiszolgáló lenne, mindegy melyik csomóponthoz érkezik a kérés, az olvasás a **legutóbbi írást adja vissza**.
>2. Rendelkezésre állás: minden kérésre **érkezik válasz**, nem biztos, hogy a legutolsó írást fogjuk olvasni
>3. Particionálástűrés: **működik a rendszer** annak ellenére, hogy a csomópontok elszakadtak egymástól és az üzenetek kiesnek vagy késnek.

>[!tip]+ Alternatívák
>
>- Egyetlen kiszolgáló -> nincs particionálás:
>	- konzisztens és rendelkezésre áll
>	- egy kiszolgáló -> nem skálázódik, SPoF
>- Több kiszolgáló -> lesz hálózati hiba -> több partíció lesz
>	- Konzisztencia VAGY rendelkezésre állás
>- Alt1: alkalmazás leállítása, amíg meg nem szűnik a particionálás
>- Alt2: konzisztencia adott időablakban

#### strong vs eventual consistency

Erős konzisztencia: a rendszer belül szinkronizál, a kívülről érkező kéréseket ez alapján szolgálja ki.

Eventual consistency: a rendelkezésre állást priorizálja a konzisztencia helyett
- Olyan adatot enged olvasni, ami nem a legfrissebb
- Inkonzisztencia ablak: terheléstől függ
- A különböző csomópontokon különböző adatverziók vannak
- DNS, Facebook üzenőfal, stb.
- Pénzügyi tranzakciók gyakran engednek 24 órát

>[!summary]+ SQL vs NoSQL
>Tipikusan SQL:
>- ACID: Atomicity, Consistency, Isolation, Durability
>- Mintha egy csomópont lenne
>
>Tipikusan NoSQL
>- BASE: Basically Available, Soft State, Eventual Consistency
>- Konzisztencia feladása a rendelkezésre állásért
>- Szenárió: pl Facebook likeok száma, névmódosítás, stb.

>[!warning]+ Kihívások
>SQL:
>- Méret
>- Merev adatmodell
>- Skálázás
>- Rendelkezésre Állás
>
>NoSQL:
>- Denormalizálás -> konzisztencia karbantartás
>- Skálázás ->tranzakcionális megkötések
>- [[Félig strukturált adatok]] -> sérülhet a típusosság
>	- [[Megjelenítési Réteg|Felhasználói felületre]] milyen adat kerül ki?
>	- Az API milyen adatokat ad tovább [[Adatvezérelt rendszerek architektúrái#**Háromrétegű architektúra**|rétegeknek]], külső szolgáltatásoknak?
>	- Több séma verzióhoz hogyan rendelünk kódot?

