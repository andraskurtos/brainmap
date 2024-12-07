---
aliases: 
tags:
  - 5semester
  - uni
  - mobweb
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-03 15:58
---
# Bootstrap

>[!question]+ Miért használjuk a bootstrapet?
>- Könnyen tanulható és használható a részletes dokumentáció miatt
>- Kiváló grid rendszer a reszponzív layout kialakításhoz
>- Nem kell mindent 0 nulláról megírni
>	- Alapvető HTML elemekre kész stílusok
>	- Számos gyakran használt komponens design
>	- Rengeteg időt megspórol
>	- 4-es verziótól flexbox alapú
>	- 5ös verziótól már a CSS grid is kipróbálható

A Bootstrap grid:

Mobile first
- Mobilra mondjuk meg az elrendezést, majd ezt a képernyő növekedésével folyamatosan felülbírálhatjuk.
- 
A képernyőt sorokra és oszlopokra bontja
- Minden sor 12 azonos méretű oszlopból áll
- A sorok száma, mérete tetszőleges
- Egy konkrét tartalmi elem több oszlopon is átnyúlhat
- A sorok magasságát nem kézzel méretezzük, hanem a legmagasabb cella határozza meg

A Grid gyökere a **container**, vagy **container-fluid** osztály. Ebbe **row** osztállyal dekorált containereket teszünk, amikbe kizárólak **col-\**** osztályú elemek kerülnek.

### A col- osztályok működése

Prefix-ekkel megadjuk, melyik töréspont fölött alkalmazza.
- Pl. a *col-sm-\** tableteken és desktopokon is érvényesül, de mobilon nem
- Ha nem adunk meg töréspontot, akkor minden felbontásnál az jut érvényre.

A specifikusabb osztály felülírja a kevésbé specifikusat
A \* adja meg, hány oszlopot foglaljon el a cella.
- Az oszlopok száma mindig 12, mobilon is
- Az alapértelmezett cellaméret 12 oszlop.

col-{md}-auto --> változó szélességű oszlop
col --> minden automatikus

A cellákba újabb **row**-t vehetünk fel, amit a **col**-ok segítségével további cellákra oszthatunk.

**függőleges igaztás:**
align-self-*
align-items-*

### Reszponzív képek:

Képek, amik kitöltik a szülők adta teret, de a képarány megtartása mellett.
Megoldás: `max-width: 100%; height: auto;`
Bootstrapben az `.img-fluid` pont ezt csinálja

## Kész komponensek

Számos komponens design-t is kapunk
pl: alert, badge,card, carousel, dropdown, modal, navbar...

- Űrlap kezelés
	- form-check, form-check-input
	- checkbox, radio
	- input-group
	- form-floating: floating label
- Témák


