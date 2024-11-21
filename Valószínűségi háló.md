---
aliases:
  - Bayes-háló
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-11-21 11:36
---

# Valószínűségi háló

>[!summary]+ Definíció
>A *valószínűségi háló* egy [[gráf]], ahol:
>1. Csomópontok: valószínűségi változók egy halmaza
>2. Csomópontok közt irányított élek:
>   Y csomópont --> X, ha Y-nak közvetlen befolyása van az X-re
>3. Minden csomópont feltételes valószínűségi tábla --> szülők hatása a csomópontra $P(X|Szülők(X))$
>4. A gráf nem tartalmaz irányított kört (= DAG)

A háló az *együttes valószínűségi eloszlás* egy leírása, avagy a [[Függetlenség#Feltételes függetlenség|feltételes függetlenségekről]] szóló állítások együttese.

A két szemlélet ekvivalens.

>[!note] Általános eljárás egy háló fokozatos megépítésére
>1. Határozzuk meg a problémát leíró változókat
>2. Határozzunk meg egy sorrendet
>3. Ameddig maradt még érintetlen változó:
>	1. Válasszuk a következő $X_{i}$ változót, és adjunk hozzá egy csomópontot a hálóhoz.
>	2. Legyen a Szülők($X_{i}$) a csomópontok azon minimális halmaza, amik már szerepelnek a hálóban és a feltételes függetlenség tulajdonságát teljesítik - húzzuk be a kapcsolatokat.
>	3. Definiáljuk $X_{i}$ csomópont feltételes valószínűségi tábláját
>
>Mindegyik csomópontot csak korábbi csomóponthoz csatlakoztathatunk = DAG lesz
>A valószínűségi háló nem tartalmaz redundáns értékeket

Egy valószínűségi háló gyakran sokkal **tömörebb**, mint az együttes valószínűségi eloszlásfüggvény. Ez a **lokálisan strukturált** (avagy ritka) rendszerek egy példája.

##### lokálisan strukturált rendszer
>[!summary] Definíció
>**Lokálisan strukturált rendszer:** egy komponense csak korlátos számú másik komponenssel van kapcsolatban közvetlenül, függetlenül a komponensek teljes számától.


## Következtetés valószínűségi hálókban

Az alapvető feladat: kiszámítani a posteriori valószínűséget a *lekérdezéses változókra*, ha a **tény** illetve **bizonyítékváltozóknak** értékei adottak.

Általában az [[Ágens]] az érzékeléséből kap értékeket a tényváltozókhoz, és más változók lehetséges értékeiről kérdez, hogy el tudja dönteni, milyen cselekvést végezzen.

### Diagnosztikai következtetés

Hatásról az okra következtetünk.

Ha adott *JánosTelefonál*, akkor kiszámíthatjuk, hogy pl P(Betörés|JánosTelefonál) = ?

### Okozati következtetés

Okról a hatásra következtetünk.
Ha adott a *Betörés*, kiszámíthatjuk, hogy pl. P(JánosTelefonál|Betörés) = ?


### Okok közti következtetés

Következtetés egy közös hatás okai között

Ha adott a *Riasztás*, akkor P(Betörés|Riasztás) = 0.376
Ha azonban Földrengés is igaz:
P(Betörés|Riasztás,Földrengés) = 0.003


### Naiv Bayes-hálók

Feltevések:
1. Kétféle csomópont lehetséges: "ok" és "következmény"
2. A következmények egymástól feltételesen függetlenek feltéve az okot

## Következtetés valószínűségi hálókban

### Irreleváns változók eliminálása

P(*JánosTelefonál|Betörés*) lekérdezés eredményét nem változtatja meg *MáriaHív* eltávolítása a hálóból.
Általában bármely **levél** csomópontot eltávolíthatunk, ami **nem célváltozó**, vagy **nem bizonyíték**. Eltávolítás után lehetnek újabb levélcsomópontok, melyek szintén irrelevánsak lehetnek.
>

>[!summary]+ DEF
> Minden változó, ami nem őse a célváltozónak, vagy egy bizonyítékváltozónak, **irreleváns** a lekérdezésre. A változó elimináló algoritmus az összes ilyen változót eltávolíthatja a lekérdezés kiértékelése előtt.

### Egzakt következtetés komplexitása

>[!summary]+ DEF
>**egyszeresen összekötött:** bármely két csp között legfeljebb egyetlen irányítatlan út van.
>**többszörösen összekötött:** bármely két csomópont között több az út.

### Algoritmusok lekérdezések megválaszolására

> [!NOTE]
> Az egyszeresen összekötött, fa gráfra *létezik lineáris komplexitású algoritmus*
> A többszörösen összekötöttre *nem létezik zárt alakú (rekurzív) algoritmus*

A számítás rekurzív hívásokból áll, X-ből indulva, a hálóban minden lehetséges úton haladva.
A rekurzió leáll a:
- ténycsomópontokon
- gyökércsomópontokon
- levélcsomópontokon

>[!summary]+ 
>Csoportosító, összevonó eljárások: átalakítják a hálót valószínűségek szempontjából ekvivalens fa gráffá.
>Sztochasztikus szimulációs eljárások: A tárgytartomány nagyon nagy számú konkrét modelljét generálják le, ami konzisztens a valószínűségi háló által definiált eloszlással.


