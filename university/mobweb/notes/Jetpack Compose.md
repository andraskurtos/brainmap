---
aliases: 
tags:
  - 5semester
  - mobweb
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 14:26
---





# Jetpack Compose

A *Jetpack Compose* egy modern toolkit UI készítéshez, amelyben a fejlesztő [[Kotlin]] kódban leírhatja a UI paramétereit, és a [[Compose Alapelvek|Compose]] motor generálja a [[Felhasználói Felület (Android)|felületet]]. Könnyű a UI frissítése az alkalmazás állapota alapján, valamint [[Erőforrások|erőforrásokat]] ugyanúgy használhat.

>[!success]+ Előnyök:
>- Kevesebb kód
>- Nincs szükség XML layoutokra
>- Nincs szükség UI widgetek készítésére
>- A UI elemek kódból készíthetőek
>- Könnyebb újrafelhasználhatóság
>- Kompatibilis a meglévő UI toolkittel.

---
## Composable függvények

Segítségükkel készíthetők [[Felhasználói Felület (Android)|UI]] elemek [[Kotlin]] kódból, alakzat és adatfüggőségek megadásával. A függvények elején `@Composable` annotációt helyezünk el. Ezek a [[Felhasználói Felület (Android)|UI]] elemek egymásba ágyazhatók lesznek.

>[!info]+ Compose UI elvek:
>A Compose az állapotokat UI elemekké transzformálja
>- Elemek "kompozíciója"
>- Elemek Layout-ja
>- Egyedi rajzolás

---

### Layout-ok

Egy **@Composable** függvény egy vagy több felhasználói felületi elemet bocsáthat ki. A rendezésre vonatkozó utasítások nélkül megeshet, hogy a Compose rosszul rendezi az elemeket. Alapértelmezés szerint a levélírás egymásra helyezi a szövegeket, így az olvashatatlanná válik. A Compose használatra kész elrendezések gyűjteményét biztosítja, és megkönnyíti az egyéni elrendezések definiálását.

Alap Layoutok:
- Column
- Row
- Box

![[Pasted image 20241015143451.png]]

![[Pasted image 20241015143627.png]]

---

### Modifier-ek

A módosítók a Composable elemek díszítésére vagy kiegészítésére szolgálnak, és elengedhetetlenek a megjelenés személyre szabásához.
Hasonló szerepet töltenek be, mint a nézetalapú elrendezések elrendezési paraméterei. A módosítók típusbiztonságot nyújtanak, és egyértelművé teszik, hogy mi áll rendelkezésre és alkalmazható egy adott elrendezéshez.

>[!note]+ Lazy Loading
>Csak olyan elemek renderelése, melyek láthatók:
>- **LazyColumn, LazyRow**
>- LazyVerticalGrid, valamint LazyHorizontalGrid
>
>Fontos: manuálisan kell importálni:
>`import androidx.compose.foundation.lazy.items`



---

## State

Az alkalmazásban az *állapot* minden olyan érték, amely idővel változhat. Minden Android alkalmazás a következőhöz hasonló állapotot jelenít meg a felhasználó számára:
- Egy SnackBar, amely megmutatja, ha egy üzenet sikeresen el lett küldve
- Vásárlási cikk és kapcsolódó részletek
- Egy animáció, amely akkor játszódik le, ha egy elemre kattintanak

A Compose deklaratív, így az egyetlen módja a frissítésnek, ha ugyanazt más paraméterekkel hívjuk meg. Ezek az argumentumok a felhasználói felület állapotának ábrázolásai, minden alkalommal, amikor egy állapot frissül, *recomposition* történik.

---
### Remember és mutableStateOf

A Composable függvények a *Remember* API-al tárolhatnak egy objektumot  a memóriában. Ez módosítható és nem módosítható objektumok tárolására is alkalmazható. A remember objektumokat tárol a kompozícióban, és elfelejti az objektumot, amikor a remember nevű komponált anyagot eltávolítják a kompozícióból.

A *mutableStateOf* létrehoz egy megfigyelő **MutableState \<T\>**-t, amely egy, a fordítási futtatókörnyezetbe integrált, megfigyelhető típus. Az érték bármilyen módosítása ütemezi az értéket olvasó összeállítő függvény recompositionjét.

A *RememberSaveable* túléli a konfigurációváltoztatást. Bár a remember segít megőrizni az állapotokat a recompositionök között, az állapot nem marad meg a konfiguráció változások során. Ehhez rememberSaveable-t kell használni.

Egy Composable, amely használja a remember-t, gyakorlatilag egy belső állapotot definiál, így az adott Composable függvény Stateful lesz.
Stateless elérése--> state hoisting

### State Hoisting

Egy minta, amivel egy Stateful Composable Stateless-é tehető
Eljárás: az állapotváltozót két függvényargumentummal váltjuk ki:
- **value: T**: az érték, amit meg kell jeleníteni
- **onValueChange: (T) -> Unit**: egy esemény, amit meg kell hívni, ha változtatni kell az értéket, de ezt kívülről kapjuk meg

>[!success]+ State hoisting előnyei:
>- Single source of truth
>- Beágyazott: Csak az állapotot nyilvántartó Composable-ök módosíthatják az állapotukat
>- Megosztható: A "kiszervezett" állapot több Composable-el is megosztható
>- Az állapot nélküli összetevők hívói dönthetnek úgy, hogy figyelmen kívül hagyják vagy módosítják az eseményeket az állapot módosítása előtt
>- Leválasztás: az állapot áthelyezhető az állapottulajdonosokhoz, pl egy ViewModel-hez.

---

## Kódszervezés

Packages:

- preview package
- screen:
	- fő képernyők
- theme:
	- design
	- színek
	- stílusok
	- stb.
- view:
	- segédnézetek, dialógusok
	- újrafelhasználható nézetek
- navigation:
	- navigációs konstansok
- root:
	- activity-k

>[!abstract]+ Definíciók
>**Initial Composition**: kompozíció létrehozása Composable első futtatásával.
>
>**Composition**: A Jetpack Compose által készített felhasználói felület leírása
>
>**Recomposition**: A kompozíciók újbóli futtatása a kompozíció frissítéséhez,  amikor az adatok/állapotok megváltoznak.


---
