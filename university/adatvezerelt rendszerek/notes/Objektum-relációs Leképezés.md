---
aliases: 
tags:
  - 5semester
  - adatvez
  - datadriven
  - uni
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
>- [[Objektum-relációs Leképezés#Öröklés|Öröklődés]]
>- [[Objektum-relációs Leképezés#Shadow információk|Shadow információk]]
>- [[Objektum-relációs Leképezés#Kapcsolatok|Kapcsolatok leképezése]]



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
## Problémák


### Shadow információk

A *shadow információk* szükségesek a perzisztencia megvalósításához. Ilyenek például a kulcsok, időbélyegek, verziószámok, stb. Az üzleti objektumba helyezni őket nem szükséges, de mégis kezelni kell valahogyan.

---

### Öröklés

Az *öröklődés* modellezésének fajtái:

1. [[Objektum-relációs Leképezés#1 - egy táblába való leképezés|Hierarchia leképezése egy közös táblába]]
2. [[Objektum-relációs Leképezés#2 - Valós osztályok leképezése táblába|Minden valós osztály leképezése saját táblába]] (akikből objektumok képződhetnek)
3. [[Objektum-relációs Leképezés#3 - Összes osztály leképezése táblába|Minden osztály leképezése saját táblába]]
4. [[Objektum-relációs Leképezés#4 - leképezés általános struktúrába|Osztályok és hierarchia szintek általános leképezése]]

>[!example]+ Példa:
>- Absztrakt Person osztály
>- Több implementáció
>- Új funkciót építünk az alkalmazásba -> új leszármazott *Executive*
>
>![[Pasted image 20241024132257.png]]

#### 1 - egy táblába való leképezés

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

#### 2 - Valós osztályok leképezése táblába

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

#### 3 - Összes osztály leképezése táblába

- Osztályhierarchiát követik a táblák
- Szülő-gyerek viszony leképezése idegen kulccsal
- Példányazonosító

![[Pasted image 20241024133254.png]]

>[!success]+ Előnyök:
>- Könnyű megérteni
>- Könnyű módosítani a szülő osztályok struktúráját

>[!danger]+ Hátrányok:
>- Összetett [[Adatbázis]] séma
>- Egy példány adatai több táblában vannak
>	- Összetettebb lekérdezések
>	- Szükséges join --> lassabb

>[!question]+ Mikor használjuk?
>- Komplex hierarchia esetén
>- Változó struktúra esetén

---

#### 4 - leképezés általános struktúrába

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

>[!abstract]- Többszörös öröklés
>Ma már nem nagyon támogatott, de pl. C++-ban igen. Itt is ugyanazok a technológiák használhatók, az általános megoldásban benne vannak.
>
>![[Pasted image 20241024134315.png]]

---
#### Melyiket használjuk?

A fenti tervezési szempontok gyakran másodlagosak. Gondoljuk át, milyen lekérdezéseket, műveleteket hajtunk végre, milyen gyakorisággal, hogy mekkora mennyiségű adattal dolgozunk, milyen ezek eloszlása. Akár tesztelhetjük és mérhetjük is a különböző alternatívák hatékonyságát.

---

### Kapcsolatok

Kapcsolatok:        \
- Asszociáció      |
- Aggregáció       |  
- Kompozíció       |
Típusok:             > --> Referenciális integritás
- [[Objektum-relációs Leképezés#Egy-egy kapcsolat|Egy-egy]]         |
- [[Objektum-relációs Leképezés#Egy-több kapcsolat|Egy-több]]         |
- [[Objektum-relációs Leképezés#Több-több kapcsolat|Több-több]]       /

Irány:           \
- egyirányú      > --> nem képezhető le
- kétirányú     /


#### Egy-egy kapcsolat:

Külső kulcs az egyik adattáblába, ez azonban az egy több lehetőséget magában hordja.

#### Egy-több kapcsolat:

Külső kulcs az egy-re.

#### Több-több kapcsolat:

Közvetlenül nem leképezhető, kapcsolótáblát használunk.

#### Kardinalitások:

Mindkét oldal kötelezőt nem célszerű leképezni. Elindulási probléma [[Adatbázis]] szinten. Számolás [[Adatelérési réteg|adatréteg]] szinten: 0, 1, több.

>[!abstract]- Rekurzió
>Másnéven reflexió, olyan kapcsolat, melynek kezdő és végpontja ugyanaz az entitás. Hasonlóan kezeljük a többi kapcsolathoz, több-több leképezésben kicsi eltéréssel.
>
>![[Pasted image 20241024135654.png]]
>
>![[Pasted image 20241024135705.png]]

>[!summary]+ Konklúzió:
>A relációs adatmodell nem tud minden kapcsolati kényszert ellenőrizni, amit az OO megközelítés elő tud írni. Példáuk egy-egy helyett egy-több előfordulhat, egy-több alesetei nem validálhatók. Erre különböző megoldásaink lehetnek, ellenőrizhetjük őket az [[Üzleti Logika Réteg|üzleti logikában]], [[T-SQL#Triggerek|triggerrel]], vagy adatbázismotor-specifikus kényszerekkel.

---

## Egyéb problémák

### Rendezett gyűjtemények

Sorrendezést adó attribútumra van szükség, hogy sorrendhelyesen olvashassuk fel (order by). Sorrend változásának hatására több rekordot is módosítanunk kell, vagy hézagos lesz a kiosztás. Elem törlésénél nem fontos újraindexelni.

---

### Osztályszintű tulajdonságok

Ezek a tulajdonságok nem kötődnek példányhoz, az egész osztályra jellemzők. Ilyenkor több lehetőségünk is van:

- *Minden tulajdonságot külön táblába teszünk:*
	- sok kicsi tábla
	- átláthatatlan
	- de: gyors!
	  
- *Minden tulajdonság ugyanabban a táblában, külön oszlopokban:*
	- gyors
	- egyszerű
	- konkurencia
	  
- *Osztályonként egy tábla, az értékek külön oszlopokban*
	- gyors
	- sok kicsi tábla
	- áttekinthetetlen
	  
- *Általános megoldás:*
	- minden tulajdonság új rekord
		- osztály
		- tulajdonságnév
		- érték
	- **Adatkonverziót meg kell oldani, vagy több oszlop**
	- Egyszerű bővíthetőség
		- Új tulajdonság -> új rekord

Az osztályszintű konstansokat is hasonlóan kezeljük.

---

## Konkurenciakezelés

A konkurenciakezelésre sajnos nincsen/ritkán van jó megoldás. Használhatunk *pesszimista konkurenciakezelést*, ( azt feltételezzük, hogy "biztos más is módosít"), ilyenkor a  módosított adatokhoz kizárólagos hozzáférést nyújtunk, zárolással, tranzakcióizolálással, várakoztatással. Vagy használhatunk optimista konkurenciakezelést is, ("más úgysem módosít"). Ha mégis, detektáljuk az ütközéseket, és feloldjuk őket.

### Pesszimista konkurenciakezelés

Ha van fenntartott kapcsolat az adatbázissal, a rekordokat zároljuk. Ezt az adatbázistranzakciók biztosítják. Ha nem tudunk kapcsolatot fenntartani az adatbázissal, pl. egy [[Adatvezérelt rendszerek architektúrái#**Háromrétegű architektúra**|háromrétegű alkalmazásban]], akkor az [[Üzleti Logika Réteg|üzleti logikai rétegben]] kell megoldani. Meg kell valósítani a kölcsönös kizárást, skálázhatóságot, és az eldobott sessionöket kezelni kell.

### Optimista konkurenciakezelés

Módosítás előtt a változtatásokat ellenőriznünk kell, majd a rekordok tartalma alapján döntést kell hoznunk. Ilyenkor vagy a rekord verzióját vizsgálhatjuk, ami adatbázis séma módosítással jár, vagy a teljes tartalmát. Ekkor meg kell őrizni a módosítás előtti tartalmat, amihez az [[Üzleti Logika Réteg|üzleti logika]] i entitásokban plusz tartalom szükséges. A módosító SQL parancsot megfelelően kell megírni, ezt az [[Adatelérési réteg|adatelérési réteg]] tipikusan segíti.

>[!summary]+ Ütközéskezelési stratégiák:
>- Az első író nyer.
>- Az utolsó író nyer.
>- Összevetjük a módosított mezők halmazát
>	--> konzisztenciát meg kell őrizni!
>- Az esetek túlnyomó részében nem egyetlen táblát érintenek a változtatások.

>[!warning]+ Ütközésdetektálás
>- Többrétegű alkalmazás esetén az adat kikerül a UI rétegbe, és az adatbáziskapcsolat bezárul.
>- Módosítási kérés esetén valahonnan *meg kell tudnunk*, mi volt a rekord eredeti állapota/verziója
>- A szerver állapotmentes -> a UI-nak kell eltárolni az eredeti adat információit
>- --> ***módosul az API***

