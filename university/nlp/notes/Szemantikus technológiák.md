---
aliases: 
tags:
  - nlp
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2025-01-07 21:41
---

# Szemantikus Technológiák

>[!NOTE]+ Lingvisztikai szemantika
>*A lingvisztikai szemantika* a nyelvek jelentésszerkezetét tanulmányozza, különösen szavakban és mondatokban.
>A *jelentés* szót különböző szövegkörnyezetekben, különböző célokra használjuk.
>- The word *perplexity* **means** 'the state of being puzzled'
>- The word *rash* has two **meanings**: 'impetious' and 'skin irritation'
>- In spanish, *espejo* **means** 'mirror'
>- I did not **mean** that he is incompetent, just inefficient
>- The **meaning** of cross as a symbol is complex
>- I **meant** to bring you my essay, but I left it at home

A szemantikát kezdetekben a nyelvtudomány részeként definiálták, a jelentés leírására. (Szavak / mondatok jelentése). 

Miért érdekes ez a nyelvtudomány számára?
Hogyan alkalmazhatjuk ezt nem természetes nyelvek leírásra és általánosabb informatikai célokra?


>[!NOTE]+ Nyelvtan
>A nyelvtan a beszélő/hallgató tudásának jellemzése. Megkérdezzük:
>"Ha egy beszélő tud egy nyelvet, mit *tud* pontosan?"
>A nyelvész feladata az, hogy jellemezze, mit kell a beszélőnek tudnia a nyelv előállításához, megértéséhez.

Célunk, hogy a számítógépek képesek legyenek értelmezni az információkat. De mit jelent értelmezni/tudni?

## Szemantika, mint nyelvtan része

A szemantika a beszélő nyelvi tudásának része, ezért a szemantika a nyelvtan része. A beszélők bizonyos előzetes ismeretekkel rendelkeznek, megértik, mire gondolnak mások, és képesek kimondani, mit gondolnak.

Egy könyvben a mondatok nagy része teljesen új lesz számunkra, *mégis képesek vagyunk megérteni*. Nyelvtudásunk jellemzéséhez le kell írni egy olyan rendszert, hogy minden új megnyilatkoztatást értelmezni tudjunk.

>[!WARNING]+ A tudásleírás problémája
>Chomsky ezt Platón által megfogalmazott problémaként azonosítja:
>*Amit hallunk/mondunk, az sokszor újdonság*
>*Hogyan tudjuk megérteni és előállítani a dolgok ilyen végtelen sokféleségét, még akkor is, ha sosem hallottuk őket?*
>
>Az 1950-es évek óta alapvető motivációja sok nyelvészeti munkának.
>
>Miben járulhat hozzá a szemantika a nyelvtudományhoz, amit más irányzatok nem vesznek figyelembe?
> - szintaktika (kifejezések struktúrája)
> - morfológia (szavak struktúrája)
> - fonológia (hangok struktúrája)

A jelentésismeret jellemzésekor az a probléma is felmerül, hogy a nyelvi tudást meg kell különböztetni a világ ismeretétől.

![[Pasted image 20250107220455.png]]

Lexikai szemantika: hogyan kezeljük a szavak és a fogalmak kapcsolatát?
Mondatok szemantikája: hogyan dekódoljuk az összetett mondatok jelentését?
lexikai&mondatok: hogyan kapcsolódik a jelentés a világhoz?

## A tudás kérdése

A szemantikai elmélet megtervezésekor  számolnunk kell a produktivitás, hatékonyság kérdésével. Nagyon sok szót (ezreket) ismerünk, és azok jelentését. Ez a mi mentális lexikonunk. Nyelvünk nyelvtani szabályait felhasználva végtelen számú mondatot alkothatunk. A mondatok jelentése az összetevő szavaik jelentéséből és kombinálásuk módjából adódik.

Hogyan írjuk le mindegyik jelentését?
*Általános tudás leírására van szükség!*

>[!NOTE]+ Kompozíciós elv
>A jelentés produktivitás megoldásának a vezérelve a kompozíciós elv.
>Egy mondat jelentése a benne található szavak és azok kombinálásainak módjától függ.
>
>Építkezzünk!

>[!QUESTION]+ Hogyan működik a szemantika?
>Lexikális szemantika:
>*A szavak jelentése más szavak kapcsolataként vannak definiálva*
>![[Pasted image 20250107221600.png]]
>Extenzionális szemantika:
>*A szavak jelentése ezek példányain keresztül vannak leírva*
>![[Pasted image 20250107221615.png]]
>Intenzionális szemantika:
>*A szavak jelentése a példányok tulajdonságai által vannak megadva*
>![[Pasted image 20250107221630.png]]
>Prototípus szemantika:
>*A szavak jelentése egy jellemző prototípuson keresztül vannak körülírva*



## Wordnet

A *Wordnet* egy szótár alapú, pszicho-lingvisztikai alapokra építő, jelentésalapú feldolgozása a lexikonoknak. Segítségével lehetséges fogalom alapú keresés lexikális adatbázisokon. 

A fogalmakat szemantikus hálóba rendezzük. A lexikális információkat a szavak jelentése alapján rendezzük, és nem a szavak formája szerint.
A szó egy asszociációs kapcsolat a szótárba gyűjtött fogalmak, és a szó alakja között. 
Lexikalizált fogalomgyűjtemény -> lexikális mátrix:
szó alakok - oszlopok
szó jelentések - sorok

![[Pasted image 20250107222057.png]]

> [!NOTE]+ Reprezentációs módszer:
> **konstruktív**: a reprezentáció tartalmazzon elégséges információt a fogalom felépítéséhez
> **differenciális**: a jelentések úgy legyenek fűzérekkel reprezentálva, hogy megkülönböztethetőek legyenek.

> [!NOTE]+ Wordnet fogalmak:
> **hipotézis**: a szinonimahalmaz egy megfelelő megközelítés egy fogalom definiálására
> **differenciális megközelítés**: a szavak jelentését reprezentálhatjuk egy szólistával


>[!SUMMARY]+ Szinonima
>Két szó *szinonima*, ha a következők fennállnak:
>- Minden szemantikus tulajdonságuk értéke megegyezik
>- Ugyanannak a fogalomnak a megjelenései
>- Kielégítik a *Leibniz-féle* helyettesítési szabályt:
>	- Ha felcseréljük a szinonimákat egy mondatban, akkor a mondat igazságtartalma nem változik.

>[!SUMMARY]+ Hiponima
>A *hiponima* olyan szókapcsolat, ahol a gyűjtő szó tartalmazza a kapcsolt szavak jelentését (alárendelés).
>
>![[Pasted image 20250107222609.png]]

>[!SUMMARY]+ Meronima/Holonima
>"Része" kapcsolat a jelentésben.
>- tranzitív és asszimmetrikus
>- egy tartalmazó fogalomhoz sok tartalmazott kapcsolódhat

>[!NOTE]+ Szókategóriák
>Főnevek:
>- Hierarchiába szervezve - több
>
>Igék:
>- Vonzat kapcsolatokon keresztül rendezve
>
>Melléknevek:
>- relációk ( pl ellentét) mentén rendezve

