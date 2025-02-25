---
aliases:
  - tranzakció
tags:
  - adatvez
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-14 10:22
---

# Tranzakciók

>[!summary]+ Tranzakció
>A tranzakció a feldolgozás logikai egysége, olyan műveletek sorozata, amelyek csak együttesen értelmesek.
>Alaptulajdonságok:
>- Atomicity
>- Consistency
>- Isolation
>- Durability

## Atomicitás és Konzisztencia

![[Pasted image 20241114102533.png]]


## Izoláció

![[Pasted image 20241114102545.png]]


## Tartósság

![[Pasted image 20241114102609.png]]


# Tranzakcióhatárok

Tranzakciót explicit indíthatunk, zárhatunk: `begin transaction --> commit/rollback`
Állíthatjuk a tranzakciós szintet
Gyakori a beágyazott tranzakciók lehetősége
Ha nincs tranzakcióban, akkor minden utasítás önmagában egy tranzakcióként fut.

## Tranzakciók izolációja

>[!summary]+ Ideális megoldás
>A tranzakciókat úgy kell végrehajtani, *mintha* egymás után történnének, és nem párhuzamosan. Csak olyan műveleti sorrend engedhető meg, melyek nem sértik az ideális ütemezést. Ha sérülne a helyes ütemezés, akkor a tranzakció várakozik. Csak olyan ütemezés engedhető meg, mely konfliktus-ekvivalens egy soros ütemezéssel -> konfliktusmentes cserékkel sorossá alakítható
>**Viszont ez az ideális ütemezés túl alacsony teljesítményű.**

Alapproblémák:
	- sok párhuzamos tranzakció
	- ideális sorrend betartása túl lassú
	- párhuzamos futtatás tipikus problémái:
		- piszkos olvasás
		- nem megismételhető olvasás
		- elveszett módosítás
		- fantom rekordok
	- **Nem csak relációs db-kben!!**
		- minden többfelhasználós, elosztott rendszerben jelentkezik


## SQL ANSI szabvány izolációs szintek

1. Read Uncommited
	- mind a 4 probléma jelentkezik

2. Read Commited
	- nincs piszkos olvasás

3. Repeatable read
	- nincs piszkos olvasás
	- nincs megismételhetetlen olvasás
	- nincs elveszett módosítás

4. Serializable
	- egyik probléma sem fordulhat elő


## Izoláció zárakkal

Zárak: egy adatnak egyetlen tárhelye van, azt védi a zár --> a többiek várnak
Egy tranzakció módosítja --> rátesz egy zárat
A többiek nem olvashatják, mert az új, piszkos adatot olvasnák

Implementációtól függően több zár is van:
	- olvasás = shared lock
	- írás = exclusive lock
	- sor / lap / táblazár

### Read Uncommited

A Read Uncommited izolációs szinten a tranzakció *nem tesz ki megosztott zárat*, más tranzakciók is írhatják az adatot. *Nem blokkolódik kizárólagos záron* a végrehajtás, tehát kiolvassa más tranzakciók nem kommitált adatát, így *piszkos olvasás fordulhat elő.*

### Read Commited

A Read Commited *olvasás esetén megosztott zárat tesz ki*, így más tranzakciók nem tudják módosítani az adatot, amíg az adott utasítás olvassa azt. Két olvasás közt más tranzakció kérhet kizárólagos zárat, és módosíthatja az adatot. Ha egy adaton kizárólagos zár van, nem tudja rátennia megosztott zárat, tehát *nem fogja kiolvasni a piszkos adatot.*

### Repeatable Read

A Repeatable Read olvasás esetén kitesz egy megosztott zárat, amit *a tranzakció végéig tart*. Így a tranzakció futása alatt mások nem tudják módosítani az adatot, tehát a megismételt olvasás ugyanazt az eredményt adja. Módosított adatokra kizárólagos zárt használ.
Ha egy adaton kizárólagos zár van, nem tudja rátenni a megosztottat -> nincs piszkos olvasás.

>[!note]+ Elveszett módosítás kivédése
>![[Pasted image 20241114104040.png]]
>
>Ez nem kapcsolódik közvetlenül a standard izolációs szintekhez.
>Tipikusan az izolációs szint implementációja határozza meg, hogy melyik szint "védi ki".
>Pl. ha mindkét tranzakció shared lockot tesz a rekordra, deadlock keletkezik, és az egyiket kilövi a rendszer az írásnál. (pl [[MS SQL]])
>Használhatunk manuális, exkluzív zárolást is.
>Legtöbb adatbáziskezelő Repeatable Readnél már megfogja.



### Serializable

Tartományzárakat tesz ki a keresési kritériumnak megfelelően, a tranzakció végéig. A keresett sorok nem módosíthatóak, nem törölhetőek és nem beszúrhatóak. Megismételt olvasás mindig ugyanazt az eredményt adja, és nincs fantomrekord.

## Nem szabványos izolációs szintek

>[!note]- [[MS SQL|MSSQL-ben:]]
>- Read commited snapshot
>- Snapshot isolation

Működése:
	- zárak helyett tároljuk el az adat korábbi verzióit
	- minden írás előtt lemásoljuk a rekordot a tempdb-be
	- a többi tranzakció a korábbi adatverziót olvassa
	- commit során érzékeljük az ütközéseket, és rollbackelünk

>[!warning]+ Snapshot vs Klasszikus
>A klasszikus 4 szint zárakkal dolgozik, pesszimista:
>	- megállítja a tranzakciót
>A snapshot verziókat hoz létre, optimista:
>	- később, commit során érzékeli az ütközést, és kilövi az egyik tranzakciót
>
> \+ Kevesebb zár, várakozás, holtpont
> \- Több tempdb használat
> 
> **Kevés írás, sok olvasás esetén lehet hasznos**


