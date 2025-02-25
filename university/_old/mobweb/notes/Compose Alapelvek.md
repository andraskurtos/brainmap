---
aliases:
  - Compose
tags:
  - 5semester
  - mobweb
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 15:07
---
w

# Compose Alapelvek


## Deklaratív paradigma

A UI elemek nem érhetőek el objektumként, a felhasználói felület úgy frissíthető, hogy ugyanazt a [[Jetpack Compose#Composable függvények|Composable függvényt]] meghívjuk különböző argumentumokkal. Az állapotot tipikusan a [[ViewModel]] tárolja, és adja a @Composable függvényeknek. Ezek felelősek az aktuális állapot átalakítására a felületen minden alkalommal, mikor egy állapot megváltozik.

---
## Recomposition

- Felhasználói interakció pl onClick event kiváltása
- Események értesítik az[[Üzleti Logika Réteg| üzleti logikát]]
- Megváltozik egy vagy több állapot (state change)
- Állapotváltozás -> minden kapcsolódó Composable függvény újrahívódik (amelyik **függ/használja** ezt az állapotot)

---

## Dinamikus felhasználói felület

Composable függvények [[Kotlin|Kotlinban]] dinamikus függvények, nem úgy mint XML layoutban, tehát a szokásos vezérlők használhatók (if, ciklusok, segédfüggvények, kotlin nyelv minden lehetősége).

---

>[!tip]+ Hatékony recomposition
>
>- A rendszer csak azokat a függvényeket vagy lambdákat hívja meg, amelyek esetleg megváltoztak, és kihagyja a többit.
>- Kihagy minden olyan függvényt vagy lambdát, amelyek nincsenek módosított paraméterei.
>
>A compose függvények 5 szabálya:
>1. A Composable függvények bármilyen sorrendben végrehajthatóak
>2. A Composable függvények párhuzamosan hajthatók végre.
>3. A Recomposition kihagyja a lehető legtöbb Composable függvényt és labdát.
>4. A Recomposition optimista,  és leálltható menet közben
>5. Egy Composable függvény nagyon gyakran is futhat/ismétlődhet, olyan gyakran is, mint egy animáció minden képkockája.

---

