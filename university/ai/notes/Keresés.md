---
created: 20240901 23:53
tags:
  - ai
  - mi
  - 5semester
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
---

# KERESÉS

A keresés a megoldás keresését jelenti, bizonyos megkötések, kényszerek és a korábban szerzett tudás, tapasztalat alapján.
A keresés lehet:

- **Nem informált**
	- ilyenkor semmilyen információval nem rendelkezünk a céllal kapcsolatban, csak valamilyen módszertan szerint haladunk a térben.
- **Informált**
	- ilyenkor különböző minőségű információink vannak a keresés lehetséges irányáról.

A keresés irányulhat egy változó értékének megtalálására, vagy például egy útvonal megkeresésére.

---
## **KERESÉSI ALGORITMUS**

A *keresési algoritmus* bemenete egy **probléma**, kimenete pedig egy cselekvéssorozat formájában előálló **megoldás**. Miután előállt a megoldás, az abban szereplő cselekvések végrehajthatók, a cél elérésének érdekében. Ezt *végrehajtási fázisnak* nevezzük.

A cél és a megoldandó probléma megfogalmazása után tehát az [[Ágens|ágens]] egy keresési eljárást indít, melynek megoldását használva vezérli cselekvéseit, majd a végrehajtott cselekvést törli cselekvéssorozatából. Miután a megoldást végrehajtotta, az ágens új célt keres.

---
### MILYEN A JÓ KERESÉSI ELJÁRÁS?

1. <u>Teljesség</u> (*completeness*): ha van megoldás, biztosan megtalálja.
2. <u>Időigény</u> (*time complexity*): mennyi idő a megoldás megtalálása?
3. <u>Tárigény</u> (*space complexity*): mennyi tárhely kell?
4. <u>Optimalitás</u> (*optimality*): ha több megoldás van, megtaláljuk-e a legjobbat? (mi a legjobb?)

---
### ÁLTALÁNOS FA ALAPÚ KERESÉS

Az általános fa alapú keresésben a lehetséges terveket, azaz a [[Állapottér reprezentáció#KERESÉSI FÁK|keresési fa]] csomópontjait kifejtjük, majd *peremet* hozunk létre a figyelembe vehető résztervek nyilvántartására. Ideálisan minél kevesebb csomópontot fejtünk ki.

```
FUNCTION FA-KERESÉS (PROBLÉMA, STRATÉGIA) RETURNS megoldás vagy kudarc

Keresési fa inicializálása a probléma kiinduló állapotával

LOOP DO
	IF nincs jelölt a kifejtéshez THEN RETURN kudarc
	Kifejtendő levél csomópont kijelölése a STRATÉGIA alapján
	IF a csomópont tartalmaz egy CÉLÁLLAPOTOT THEN RETURN MEGOLDÁS
	ELSE csomópont kifejtése és az eredmény csomópontok hozzáadása a keresési fához
END
```

**Központi kérdés: melyik perembeli csomópontot fejtsük ki?**

---
## KERESÉSI STRATÉGIÁK

- Mire vagyunk képesek? (saját modell)
- Milyen körülöttünk a környezet? (cél-, és távolsági modellek)

--> **[[Neminformált Keresés|Neminformált keresések]]** (gyenge vagy vak keresések)
- tudjuk: hogy néz ki a célállapot
- egyáltalán nem: milyen költségű a célállapotba vezető út.

--> **[[Informált Keresés|Informált keresések]]** (heurisztikus keresések)
- tudjuk: hogy néz ki a célállapot
- jó becslésünk van arra, hogy: milyen költségű lehet a célállapotba vezető út.

---
