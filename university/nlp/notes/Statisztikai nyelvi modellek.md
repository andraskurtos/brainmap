---
aliases: 
tags:
  - 5semester
  - uni
  - nlp
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-25 17:41
---


# Statisztikai nyelvi modellek

A statisztikai nyelvi modellekben statisztikai eszközökkel próbáljuk értelmezni és hasznosítani a természetes nyelvi szövegeket. Általában szöveges dokumentumokkal dolgozunk, melyeket feldolgozunk, hasonlóan a [[Információkeresés|kereséshez]], majd statisztikai elemzők segítségével különböző modelleket építünk.

## A keresési index további felhasználásai

Az index egy gyakorisági tényezővel kiegészítve már *statisztikai adathalmazként* funkcionál egy dokumentumhalmazról, ami segítségünkre lehet a nyelvi modellek alkotásában. Ez a keresésen kívűl rengeteg célra használható, lásd a példák közt.

>[!example]+ Példák:
>- Témaelemzés
>	- szavak <-- témakörök
>- [[Hangulatelemzés]]
>	- szavak <-- érzelmek
>- Szerzőségvizsgálat
>	- szókészletből
>- Nyelv meghatározása
>	- szókészletből

### Klasszifikációs feladatok

Meghatározunk kategóriákat, melyekhez jellemzőket fogalmazunk meg a szavak előfordulásának gyakoriságának alapján, majd statisztikai vizsgálat alapján eldöntjük, melyik kategóriába tartozik az adott dokumentum. Gyakran bináris kategóriák, pl: kedvelt-nem kedvelt.

## A nyelv gépi reprezentálása

>[!summary]+ [[Alfabéta]]
>Az alfabéta avagy *karakterkészlet* jelek véges halmaza, melyek a nyelv építőelemeiként funkcionálnak. Ilyenek a betűk \[A-Z\], írásjelek, whitespacek, stb. ASCII, Unicode, stb.

>[!summary]+ [[Füzér]]
>A *füzérek* az alfabétából építhetőek fel, ezek gyakorlatban a szavak és a mondatok. Véges, de nem korlátos hosszúságúak. A *szó* a legkisebb, **önálló jelentéssel rendelkező**, más szavaktól elhatárolt füzér. A *mondat* szavak valamely módon behatárolt / **összekapcsolódó sorozata**.
>
>szó --> mondat --> dokumentum --> korpusz

>[!summary]+ [[Nyelv]]
>A *nyelv* az összes lehetséges füzér (ha véges), vagy valamely modell által generált füzérek halmaza.

A *nyelvi modell* eldönti, hogy egy adott füzér a nyelv része-e, vagy generál egy nyelv részét képező füzért. Ezek lehetnek formális modellek (szabálygyűjtemény, véges automata, stb.), valószínűségi modellek (statisztikai adatokra támaszkodó eloszlásfüggvények), neurális hálózatok (adatokból tanult hálózati modellek), stb.



## A statisztikai nyelvi modellek alapjai

>[!question]+ Mit tudunk megfigyelni?
>- adat
>	- korpusz
>		- pl Brown Corpus, Wikipedia, Web
>	- szótár
>	- nyelvészeti tudás
>	- stb.
>- tulajdonságok
>	- karakterfüzérek (betűk, szavak, stb) számossága
>	- szófaj, morfológia, jelentés, ...
>	- szókapcsolatok, nyelvtan, mondatszerkezetek, ...

### Természetes nyelvek statisztikai tulajdonságai

Alapkérdések:
- hány különböző szó fordul elő a szövegben?
- adott méretű szöveggyűjtemény a szöveg mekkora részét fedi le?
- mekkora méretű korpusz kell a lefedés meghatározásához?

>[!summary]+ [[Zipf-törvény]]
>$$
>f\cdot r=\text{konstans}
>$$
>
ahol:
> $r\space-$ a szó gyakoriság-sorrendbeli rangja
> $f\space-$ a szó gyakorisága
> 
> ![[Pasted image 20241025190858.png]]

>[!summary]+ Kollokáció
>A kollokáció a szavak együttes előfordulása, melynek a szavakon túlmutató jelentőssége lehet.
>
>pl: édes néném, bakot lő

>[!warning]+ Szó kontextusa
>Szó környezetében előforduló szavak vizsgálata, ez is fontos lehet, például [[homonimák]] kezelésére (*vár*).

## NLP Pipeline

Avagy hogyan építjük fel az NLP modellt?

1. **Tisztítás** - A forrásszöveg lényegtelen, zavaró részeinek eltávolítása
2. Részegységekre (bekezdések, mondatok) bontás
3. Elemi egységekre (szavak) bontás
4. Irreleváns tokenek eltávolítása