---
aliases: 
tags:
  - 5semester
  - adatvez
  - ai
  - datadriven
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-02 16:34
---

# Objektum-Relációs Leképezés


Az O/R leképezés feladata az üzleti objektumok leképezése relációs adatmodellre, ezzel az [[Adatelérési réteg|adattárolás]] és az [[Üzleti Logika Réteg|üzleti folyamatok]] összekötése. 

>[!warning]+ Problémák:
>- Eltérő koncepciók
>- Öröklődés
>- Shadow információk
>- Kapcsolatok leképezése

## Alapötlet

Az alapötlet szerint az osztályokat táblákba, az adattagokat oszlopokba, a kapcsolatokat pedig foreign keyekbe képezzük le.
![[Pasted image 20241024131606.png]]

Ezzel is adódnak azonban problémák.

>[!warning]+ Problémák:
>- Összetett mezők:
>	- Vásárló
>		- Cím (irányítószám, utca, város)
>-  Eltérő adattípusok --> konverzió
>  
>  ![[Pasted image 20241024131758.png]]

---

## Shadow információk

A *shadow információk* szükségesek a perzisztencia megvalósításához. Ilyenek például a kulcsok, időbélyegek, verziószámok, stb. Az üzleti objektumba helyezni őket nem szükséges, de mégis kezelni kell valahogyan.

---

## Öröklés

Az *öröklődés* modellezésének fajtái:

1. Hierarchia leképezése egy *közös táblába*
2. Minden *valós osztály* leképezése saját táblába (akikből objektumok képződhetnek)
3. *Minden osztály* leképezése saját táblába
4. Osztályok és hierarchia szintek *általános leképezése*

>[!example]+ Példa:
>- Absztrakt Person osztály
>- Több implementáció
>- Új funkciót építünk az alkalmazásba -> új leszármazott *Executive*
>
>![[Pasted image 20241024132257.png]]

### 1 - egy táblába való leképezés

- Összes attribútum felsorolása a hierarchiát bejárva
- Típus azonosítás
	- Egy oszlopban kódolt értékkel
	- vagy IsCustomer, IsEmployee, stb oszlopokkal.
- Bővítés kezelése: új attribútumok felvitele

![[Pasted image 20241024132502.png]]

>[!success]+ Előnyei:
>- Egyszerű
>- Könnyű új osztályt bevezetni a hierarchiába
>- Objektum példány szerepének változása könnyen követhető

>[!error]+  Hátrányai:
>- Helypazarlás
>- Egy osztály változása miatt az összes tárolása megváltozik
>- Komplex struktúra esetén nehezen áttekinthető
>- A kötelezőségi kényszer nem valósítható meg az adatbázisban

>[!question]+ Mikor használjuk?
>- Tervezési idejű tényező
>- Egyszerű hierarchiák esetén

---

### 2 - Valós osztályok leképezése táblába

- Osztályonként egy tábla
- Osztály összes attribútumának eltárolása
- Példányazonosító
- Változás követése
	- Új osztály --> új tábla
	- Attribútum változás --> hierarchia szint mentén végig kell vinni

>[!success]+ Előnyök:
>- Átláthatóbb
>- Jobban illeszkedik az objektum modellhez
>- Gyors adatelérés

>[!error]+ Hátrányok:
>- Osztály módosítása több táblát is érinthet
>- Több szerepet betöltő példányok kezelése
>- Employee --> Executive
>- Employee és Customer

>[!question]+ Mikor használjuk?
>-  Ritkán változó struktúrák esetén

---

### 3 - Összes osztály leképezése táblába

- Osztályhierarchiát követik a táblák
- Szülő-gyerek viszony leképezése idegen kulccsal
- Példányazonosító

![[Pasted image 20241024133254.png]]

>[!success]+ Előnyök:
>- Könnyű megérteni
>- Könnyű módosítani a szülő osztályok struktúráját

>[!danger]+ Hátrányok:
>- Összetett [[adatbázis]] séma
>- Egy példány adatai több táblában vannak
>	- Összetettebb lekérdezések
>	- Szükséges join --> lassabb

>[!question]+ Mikor használjuk?
>- Komplex hierarchia esetén
>- Változó struktúra esetén

---

### 4 - leképezés általános struktúrába

- Metadata driven megoldás
- Általános séma
	- Tetszőleges hierarchia leírható
	- Független konkrét osztályoktól
		- Osztály hierarchia --> metaadat
		- Osztály példányok --> Attribútumok manifesztálódása

![[Pasted image 20241024133844.png]]

>[!success]+ Előnyök:
>- Flexibilis
>- "Bármi" leírható benne

>[!error]+ Hátrányok:
>- Elsőre nehéz "megemészteni"
>- Nehéz "összeszedni" egy objektum példány adatait
>- Nagy adatmennyiség esetén nem hatékony

>[!question]+ Mikor használjuk?
>- Komplex alkalmazások
>- "Minden változhat" akár futási időben is.

---

>[]

