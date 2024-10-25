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
>	- szókapcsolatok, nyelvtan