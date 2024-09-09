---
created: 20240901 23:53
tags:
  - uni
  - ai
  - 5semester
  - lecture
  - mi
cssclasses:
  - center-titles
  - image-borders
---

# Mesterséges Intelligencia 1. előadás

## 1 - [[MI tárgykövetelmények|Tárgykövetelmények]]

#### Előadás:

- Minden héten szerda 10:15-12:00
- Páratlan hetenként hétfő 14:15-16:00

Kivéve:
- szept 30 nincs előadás SCH QPA miatt
- okt 23

Előadó: Dr Hullám Gábor
		hullam.gabor@mit.bme.hu


MIT házi feladat portál: https://hf.mit.bme.hu
Laborbeosztás, jelentkezés itt
ZH eredmények is


___

#### Követelmények:

- 1 ZH
- 7 Labor (1 labor 1 pont)
- Összpontszám: **100 = ZH_1 (33p) + Labor (7p) + Vizsga (60p)**
- Minimum követelmények:
	- ZH 13p
	- Vizsga 24 p
	- 4 sikeres labor
- Jegyek: 
	- 41-54 *2*
	- 55-69 *3*
	- 70-84 *4*
	- 85+ *5*

#### Labor infók:

- Első labor: mindenkinek hozzárendelt időpont
- Többi:
	- jelenléti vagy online
	- dinamikus helyfoglalás
	- 1 laborgyak 2 héten keresztül

____


## 2 - Mit értünk intelligencia alatt?

### Az intelligencia dimenziói:
![[Pasted image 20240904123724.png]]

### Miért van szükségünk mesterséges intelligenciára?

- segít megismerni az emberi kognitív folyamatokat
- emberi szakértők kiegészítése, támogatása
- emberi képességek támogatása, kiegészítése, kiterjesztése

- **Muszáj: rendelkezésre álló adat és tudás meghaladja az emberi feldolgozó/felfogóképességet**
- Olcsóbb, rugalmasabb, mint egy emberi szakértő (:|)


___


#### Mérnöki szempontok:

- megoldható és kivitelezhető problémák
- hogyan elemezzük a problémákat, melyek MI alkalmazását igénylik?
- hogyan specifikáljuk?
- hogyan rögzítsük és tartsuk karban a tudást?
- milyen architektúrát használjunk?
	- tudás reprezentáció, menedzsment
	- értékelés, következtetés
	- tanulás
	- keresés
	- ...


______

### Lehetséges MI megközelítések

Emberi módon cselekvő MI <-> Racionálisan gondolkozó MI

##### **Nem feltétlenül az emberi vagy természetben elterjedt mód a legjobb, ha valamilyen célt el akarunk érni**

- Emberi módon gondolkozó MI: **kognitív modellezés**
	kognitív tudományok, biológia
- Emberi módon cselekvő MI:
	- tudásábrázolás
	- következtetés
	- természetes nyelvű kommunikáció
	- tanulás
- Racionálisan gondolkozó MI: **logika, következtetés**
	- *Nem minden intelligens viselkedés írható le tisztán logikai kifejezésekkel :(*
	- *Miről kell gondolkodni, mi a célja?*
- Racionálisan cselekvő MI: **a *megfelelő* dolgot teszi a feladat megoldásához**
	- A rendelkezésre álló információ alapján maximalizálja a *teljesítményt*
	- Kihívás: 
		- rendelkezésre álló információ többnyire nem teljes
		- a teljesítmény mérése hogyan történjen?


## 3 - Az MI korszakai:

- 1930 Univerzális számítási modell: Turing-gép, Univerzális Turing-gép, Church-Turing hipotézis, Zuse, Neumann
- 1943 McCulloch & Pitts: *Bináris kapcsolati agymodell*
- 1950 Turing: *Computing Machinery and Intelligence*
- **1956** Dartmouth találkozó: *Artificial Intelligence* megnevezés elfogadása
- 1950s Korai MI programok: sakk, tételbizonyítás

____

#### **Számítás (keresés) alapú MI:
- A fizikai szimbólumrendszer hipotézise: A. Newel & H.A.Simon (1976): *"A physical symbol system has the necessary and sufficient means for general intelligent action"*
- 1966-73 Számítási komplexitási korlátok a keresésben, elméleti korlátok a neurális hálózatokban.
- 1969 - 79 Tudásalapú szakértői rendszerek

#### **Tudásalapú MI**:
- 1986 Neurális hálózatok újbóli megjelenése
- 1988 Valószínűségi szakértői rendszerek
- 1995 Gépi tanulás gyors fejlődése
#### **Adatvezérelt MI** (2005-15)
#### **Autonóm tanulás alapú MI** (2012-20)
#### **Nagy nyelvi modellek - AGI(?)** (2020-)
Agi = Mesterséges Általános Intelligencia

___

### Az MI jósolt korszakai:
#### **Gyenge/szűk MI:**
- intelligencia egy specifikus területen
#### **Erős MI:**
- AGI: élet számos területén intelligensen viselkedik
- Gondolkodik: (Human-Level AI) absztrakt gondolkodás, következtet, összetett koncepciókat megért, tanul
#### **Szuperintelligencia:**
- Intelligensebb az emberi elméknél, tudásban, kreativitásban, problémamegoldásban

![[Pasted image 20240904125726.png]]

___

### Érdekességek:
### **IBM Grand Challenge** 
 - 1997: *Deep Blue* legyőzi Kasparovot sakkban
 - 1996-2006: *Blue Gene*, fehérje struktúra predikció
 - 2011: *Watson* beszédértés, következtetés, játék
### **Számítógépes látás (YOLO)**
- you only look once: valós idejű, teljeskörű objektumfelismerés
- mobiltelefonon
### **Egyebek**
*Orvosi döntéstámogató és szakértői rendszerek*
*Szépségverseny zsűri (Beauty AI)* - (EZ FIX RASSZISTA VOLT XD
)
*Festői stílus reprodukciója*
*AI-art :(*


___


## 4 - Mi ezekben a rendszerekben a közös?

- --> információ ... információbegyűjtés eszközei: **szenzorok**
- <-- fizikai hatás ... hatáskifejtés eszközei: **beavatkozók**
- Ami kívűl: *környezet*
- Ami belül: *rendszer*
![[Pasted image 20240904130726.png]]

___

Ágens környezetének $s_{k}(t) \in S_{K}$ állapotai vannak
Ágens magának is $s_{Á}(t) \in S_{Á}$ állapotai vannak

$$
S_{Intelligens rendszer} \subseteq S_{K} \times S_{Á}
$$
![[Pasted image 20240904131401.png]]

### Ágens:
- Az ágens egy entitás, ami érzékel és cselekszik
- Absztrakt módon megfogalmazva egy függvény, ami az érzékeléseket képzi le cselekvésre
- Minden környezethez és feladattípushoz a lehető legjobb teljesítményű ágenst keressük.

___

#### **cél és trajektória:**
- Az ágens célja a környezetének meghatározott állapotát elérni vagy megvalósítani, ami számára kívánatos $S_{cél}$
- Érzékelés és cselekvés közben az ágens egy *trajektória* mentén halad
- Trajektóriák nem egyformák
- Egy ügyes ágens hatékony trajektóriát választ.

___

#### **racionális cselekvés**
- == cél felé irányuló cselekvés
- **Intelligens ágens**: racionális módon választja meg a cselekvéseit és a célállapotait sikeresen éri el
- A tökéletes racionalitás lehetetlen, számítási szükségletek túl nagyok.
- **korlátozott racionalitás**: megfelelően cselekedni, miközben az összes számításra nincs elég idő

___

## 5 - Mitől függ és mennyire mérhető az intelligencia?
- Kell egy mechanizmus, hogy jó trajektórián tartsa az ágenst, amíg különbség van a pillanatnyi és a célállapot között.
- Ágens feladata **érzékelésből kiszámítani a cselekvést**, de!
	- érzékelések függenek az érzékelőktől
	- cselekvések függenek a beavatkozóktól
	- a számítás módja függ az ágens felépítésétől
- A cselekvés kiszámításának *ügyessége* kapcsolatba hozható a rendszer intelligenciájával

____
#### **környezet hatása az intelligenciára:**
- A szükséges intelligenciát befolyásolja a környezet
- A legnehezebb a *nem hozzáférhető, nem epizódszerú, dinamikus, nem determinisztikus és folytonos, többágenses* környezet.
- Nehezebb környezet összetettebb intelligenciát igényel.

- Valós helyzetek legtöbbje olyan bonyolult, hogy gyakorlati okokból nem determinisztikusnak kezelendő
- **Ágens ellenségei:**
	- véges erőforrásai
	- információhiány érzékeléskor
	- környezet változékonysága

_____
## 6 - Összefoglalva:
Informatikában:
- Az intelligencia egy *tervezhető* és *skálázható* rendszer-attribútum.
- intelligencia révén *igényes és újszerű szolgáltatásokat* valósítunk meg
- egy informatikusnak tudnia kell tervezéskor a rendszer intelligenciájával *gazdálkodni*


----

***20240904***

