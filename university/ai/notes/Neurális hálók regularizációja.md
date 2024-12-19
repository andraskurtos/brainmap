---
aliases: 
tags:
  - ai
  - mi
  - uni
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-17 18:07
---
# Neurális Hálók Regularizációja

>[!question]+ A neurális háló tanításának kérdései
>- Mekkora hálózatot válasszunk?
>- Hogyan válasszuk meg a tanulási tényező, $\alpha$ értékét?
>- Milyen kezdeti súlyértéket állítsunk be?
>- Hogyan válasszuk meg a tanító és tesztelő mintakészletet?
>- Hogyan használjuk fel a tanító pontokat, milyen gyakorisággal módosítsuk a hálózat súlyait?

## Mekkora hálózatot válasszunk?

Ennek meghatározásához nincs egzakt számítás.

### Kiindulás egy nagyobb hálózatból

Egy nagyobb hálózatból indulunk ki, majd a hálózatban megmutatkozó redundanciát csökkentjük minimális értékre.
- pruning technikák
- regularizáció

$$
c_{r}(w)=c_{w}+\lambda \sum_{i,j}|w_{ij}|
$$
### Kiindulás egy kisebb hálózatból

Egy kisebb hálózatból indulunk ki, majd **fokozatos bővítéssel** (processzáló elemekkel, ill. rétegekkel) vizsgáljuk, meg tudja-e oldani a feladatot.

**A két megközelítés nem feltétlenül vezet azonos eredményre.**

---
## Hogyan válasszuk meg $\alpha$ értékét?

Erre sincs egyértelmű módszer.
### Változó, lépésfüggő $\alpha$ alkalmazása

A kezdeti nagy értékből kiindulva m lépésenként csökkentjük $\alpha$ -t.

### Adaptív $\alpha$ alkalmazása

$\alpha$ változását attól tesszük függővé, hogy a súlymódosítások csökkentik-e a kritériumfüggvény értékét.


---

## Milyen kezdeti súlyértéket állítsunk be?

Nincs egyértelmű módszer.
A priori ismeret hiányában a véletlenszerű súlybeállítás a leginkább megfelelő. Az egyes súlyokat az egyenletes eloszlású valószínűségi változót különböző értékeire választjuk.

**Szigmoid aktivációfüggvénynél kerüljük el, hogy már a tanítás elején telítéses szakaszra kerüljön a rejtett rétegek kimenete.**
--> minél nagyobb a hálózat, annál kisebb véletlen értékek választása a célszerű

---

## Hogyan válasszuk meg a készleteket?

Nagy számú mintánál nem jelent problémát akár három egyenlő részre osztani az adathalmazt. A legtöbb valós probléma esetén azonban sosem elég a minta.

Létezik elméleti minimum mintaszám és gyakorlati minimum mintaszám, korábbi tapasztalatok alapján.

Keresztkiértékelési eljárásokat alkalmazunk.

Ökölszabály: tanító 80%, validációs 10%, teszt 10%
A particionálás többször is elvégezhető

### K-keresztvalidáció

Adathalmaz particionálása k darab részre, a tanulásnál k-1 lesz a tanító, k-adik a teszt adathalmaz. K-szor ismételt tanítás.

ha validációs is kell: k-2, k-1, k

### Leave-one-out kereszvalidáció

Mindig egyetlen minta a teszt, az összes többi tanító.

---

## Hogyan használjuk fel a tanító pontokat?

Pontonként:
- súlymódosítás tanítópontonként

Kötegelt eljárás szerint
- súlymódosítás k db tanítópontonként vagy
- csak a teljes tanító készlet (epoch) felhasználása után

---

## Hogyan védekezzünk túltanulással szemben?

Akkor lép fel, ha a tanító adathalmaz mintáira már nagyon kis hibájú válaszokat kapunk, de a validációs adathalmazra egyre nagyobb hibával válaszol a hálózatunk.

### A bias és a variancia kiegyensúlyozása

A regularizálás segíthet kiegyensúlyozni a
- modell torzítása (alulilleszkedés)
- és a modell varianciája (túlilleszkedés) között.

Fordított kapcsolat áll fenn a torzítás és a variancia között. Amikor az egyik csökken, a másik hajlamos növekedni, és fordítva.


### Modell torzítás

A bias azokra a hibákra utal, amelyek akkor fordulnak elő, amikor megpróbálunk egy statisztikai modellt valós adatokra illeszteni.

- A modell nem képes megtanulni a mintázatokat a rendelkezésre álló adatokban, ezért rosszul teljesít.
- Ha túlságosan leegyszerűsített modellt használunk az adatokhoz való illesztéshez, akkor valószínűbb, hogy a magas torzítás helyzetével szembesülünk.

### Modell variancia

A **variancia** azt a hibát jelenti, amely akkor fordul elő, amikor a modell által korábban nem látott adatok felhasználásával próbálunk előrejelzéseket készíteni.

**Magas variancia** akkor fordul elő, amikor a modell megtanulja az adatokban rejlő zajt.

### Torzítás-variancia kompromisszum

Fordított kapcsolat áll fenn a torzítás és a variancia között, amikor az egyik csökken, a másik hajlamos növekedni, és fordítva. A túl egyszerű, nagy torzításos modellek nem rögzítik a mögöttes mintázatokat, míg a túl összetett, nagy varianciájú modellek túlilleszkednek az adatok zajához.

## Regularizáció

- Early stopping
- Adat augmentáció
- L1 és L2 regularizáció
- Zaj adása bemenethez/címkékhez/gradienshez
- Dropout

### Early Stopping

Ha a hiba/veszteség túl alacsony lesz, és tetszőlegesen közelíti a nullát, akkor a hálózat biztosan túlilleszkedik a tanító adathalmaz. Ezért, ha meg tudjuk akadályozni heurisztikusan, hogy a hiba/veszteség tetszőlegesen alacsony legyen, a modell kevésbé valószínű, hogy túlilleszkedik a tanító adathalmazon és jobban fog általánosítani.

Egy másik módja annak, hogy tudjuk, mikor kell megállni:
- a hálózat súlyainak változásának figyelése
- legyen $w_{t}$ és $w_{t-k}$ és $t-k$ epochok súlyvektorai
- kiszámítjuk a különbségvektor L2 normáját
- ha ez a mennyiség kisebb, mint $\epsilon$, akkor leállíthatjuk a tanítást: $||w_{t}-w_{t-k}||_{2}<\epsilon$

### Adataugmentáció

Az adataugmentáció egy olyan regularizációs technika, amely segít a neurális hálózatoknak jobban általánosítani azzal, hogy változatosabb tanítómintáknak teszi ki azt.

Mivel a mély neurális hálózatoknak nagy tanító adathalmazra van szükségük, az adataugmentáció akkor is hasznos, ha nem áll rendelkezésünkre elegendő adat egy neurális háló képzéséhez.

### L1 és L2 regularizáció

Általában az Lp normák (p>=1 esetén) a nagyobb súlyokat büntetik. Arra kényszerítik a súlyvektor normáját, hogy kellően kicsi maradjon. Az n-dimenziós térben az xx vektor Lp normája a következő:

$$
Lp(x)=||x||_{p}=\left( \sum_{i=1}^{n}|x_{i}|^{p} \right)^{1/p}
$$

L1 norma: Ha p=1, akkor L1 normát kapunk, ami a vektor összetevői abszolút értékeinek összege:

$$
L_{1}(x)=||x||_{1}=\sum_{i=1}^{n}|x_{i}|
$$
L2 norma: Ha p=2, akkor L2 normát kapunk, ami a pontnak az origótól való euklidészi távolsága az n-dimenziós vektortérben:

$$
L_{2}(x)=||x||_{2}=\left( \sum_{i=1}^{n}|x_{i}|^{2} \right)^{1/2}
$$

### L1 regularizáció

Legyen $L$ és $\tilde{L}$ a veszteségfüggvények regularizáció nélkül, illetve regularizációval. Az L1 regularizált veszteségfüggvény a következő:

$$
\tilde{L}(w)=L(w)+\alpha||w||_{1}
$$
ahol $\alpha$ regularizációs konstans
Ha a súlyok túl nagyok lesznek, a második kifejezés növekszik. Mivel azonban az optimalizálás célja a veszteségfüggvény minimalizálása, a súlyoknak csökkennie kell.

Különösen az L1 regularizáció segíti elő a súl=ymátrix ritkaságát azáltal, hogy a súlyokat nullára kényszeríti, ezáltal jegykiválasztást is végez.

L2-vel:

$$
\tilde{L}(w)=L(w)+\frac{\alpha}{2}||w||_{2}^{2}
$$


