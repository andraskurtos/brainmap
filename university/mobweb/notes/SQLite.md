---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 15:40
---





# SQLite

Az android alapból tartalmaz egy teljesértékű relációs adatbáziskezelőt, az SQLite képében. Strukturált adatok tárolására ez a legjobb választás. Alapból nincsen objektum-relációs réteg felette, nekünk kell meghatároznunk a sémákat, és megírni a queryket.

>[!info]+ Android SQLite jellemzői
>- Standard relációs adatbázis szolgáltatások
>	- SQL szintaxis
>	- Tranzakciók
>	- Prepared statemetn
>- Támogatott oszloptípusok:
>	- TEXT
>	- INTEGER
>	- REAl
>- Az SQLite nem ellenőrzi a típust adatbeíráskor
>- Az SQLite adatbáziselérés fájlrendszerelérést jelent, ami miatt lassú lehet.
>- Adatbázis műveleteket érdemes aszinkron módon végrehajtani (pl Coroutine-ok vagy Thread)

>[!bug]+ SQLite debug
>Az [[Android|android]] SDK tools mappájában 