---
aliases: 
tags:
  - 5semester
  - client
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-19 17:43
---

# Teljesítmény

A szoftver le- / betöltési ideje, parseolási, JIT fordítási ideje, futási ideje, mérete mind-mind a teljesítmény mérőszámai. Főleg a [[Progressive Web App|kiadott csomagnál]] fontos, de fejlesztés közben is szempont.


## Mérőszámok

- Első rajzolás (FP - first paint)
	- amikor bármi változást látunk
- Első tartalom rajzolás (FCP - first contentful paint)
	- amikor az első HTML-beli elemet látjuk
- DOMContentLoad esemény
	- Minden globális függvény lefutott
	- A HTML betöltve, a DOM felépítve
- Load esemény
	- Minden betöltve, CSS is
- Oldal használható (TTI - time to interactive)
	- látható az oldal és reagál 50ms alatt
	- ez a legutolsó és legfontosabb mérőszám

*Google Ad Network mérése alapján ha az oldal 3 mp alatt nem töltődik be, a felhasználók 53%-a elhagyja az oldalt.*

## Optimalizáció

- [[Progressive Web App#Cache API|Caching]]
- Gyors/kicsi könyvtárak használata
- Server Side Rendering, pl Next.js
- Csomagolás (bundling)
- Lazy Loading
- Média formátumok (webp)
- Tömörítés
- HTTP/2, HTTP/3

## Csomagolás

### Fordítás

Pl. kód átalakítása [[JavaScript|JS]] -re. Például: .ts, .tsx, .jsx.
Gyors művelet:
- forrás és cél formátum nagyon hasonló
- nincs szükség strukturális átalakításra
	- de szintaktikai ellenőrzés kell

>[!NOTE] Source map: app.js.map
>A fordítás miatt fontos eltárolni, az adott kódrészlet hol volt az eredeti kódban
>- breakpointokhoz, más debug eszközökhöz szükséges
>
>Többszörös átalakítást támogatni kell
>- TS=>JS=>bundle

### Bundling

Több fájl egymás után másolása, majd modulokra bontása.

Cél:
- egyesítés után kevesebb fájlt kell letölteni
- modulok kialalítása, hogy ne egyben töltődjön le a teljes csomag
- általában: a kiadott csomag függetlenné tétele a fejlesztés alatt kialakított struktúrától

Modul rendszert át kell alakítani
- vagy egy kimeneti fájl esetén meg kell szüntetni.

>[!NOTE] ES bundlers
>A modern csomagolók csak publikáláshoz hoznak létre modulokat/csomagokat, debug módban egyesével töltik a fájlokat.
>A böngésző feladata, hogy ezeket mind betöltse, és több ezer esetén is viszonylag gyors. Nagyobb projektnél hot reload esetén a Vite nagyságrendekkel gyorsabb, mint a WebPack

### Tree Shaking

Felesleges kód eltávolítása
- Megtalálni a nem használt függvényeket
- problémás osztályok esetén
- általában is nehéz, ha nem arra van optimalizálva egy könyvtár
	- nem talál meg mindent
Célok
- A saját kódunk kisebbé tétele
- A használt könyvtárak kisebbé tételére
	- vagy akár teljes kiszűrésére

Sajnos a cél elérése nem garantált
- saját kódunkban általában kevés nem használt függvény van.
- külső könyvtárak esetén hasznos, ha úgy vannak megírva.
- számos könyvtár jön customize formában

### Minify

Nevek lecserélése rövidre
Célok:
- kisebb fájlméret
	- jelentős méretcsökkenés (1:3, 1:4)
	- Tömörítve nem annyira nagy a hatás, de úgy is jelentős
- gyorsabb
	- letöltés: cache esetén kicsi a hatása, de nem nulla
	- betöltés: itt is számít
- nehezebb olvasni mellékhatás
	- visszafejtést, megértést nehezíti (obfuscate)
	- ha ez nem cél, akkor adunk mellé fejlesztői verziót.

A CSS-t is lehet Minify-olni, de ennek kicsi a hatása, nehéz debuggolni, nem triviális a megoldás.


## Lazy Loading

### Modulok

A legjobb módszer csomagokra bontani a kódot
Ez nem triviális feladat
Értelmes csomagokat kell kapnunk
Például hiába van szétszedve, ha a fő oldalhoz mindegyik kell

Import és Export használata
Modul betöltő automatikusan megoldja
- Fordító paramétere, hogy melyiket használjuk
- Lehet használni a naiv modulkezelőt
	- Ez már széleskörben támogatott

>[!NOTE] Kimeneti formátumok
>- CommonJS - szerver oldali Node.js
>- AMD - require.js
>- UMD - mindkettő + globális változó
>- Natív ES modul - ezt használjuk új projekteknél.

#### Natív modulok

Csak ES6-tól van
- böngészőre bízzuk a betöltést
- ez gyorsabb, mint a [[JavaScript|JS]] megoldások
- Tree-shaking működik

Jelenleg csak Rollup tud ilyen kimenetet adni
	Vite ezt használja, ha dinamikus importot írunk.

*Hiába gyorsabb több száz fájl esetén lassul*
- nem publikálhatjuk az összes fájlt
- továbbra is kell csomagoló


*Code Splitting:*

Sima import esetén egybe fordul:

```javascript
import {useVariable} from "./Variable"
```

Dinamikus import esetén külön modulba fordul:

```javascript
let mod = await import("react-google-charts");
```

- saját kódot is tudunk tenni másik modulba
- nekünk kell gondoskodni a betöltés megfelelő időzítéséről

#### Külső könyvtárak

Itt is működik a modul módszer:
- de lehetséges, hogy nincs így csomagolva

*Késleltetni lehet a betöltést*
- async és defer: aszinkron tölti a scriptet
	- párhuzamosít, így javít a TTI-n
- Manuálisan csak gombnyomásra betölteni
- Prediktív módszerek
	- Pl. elindítani a betöltést mouseover-re
	- Vagy amikor a gomb bejön a a képernyőre

#### Képek és egyéb tartalom

Ami nem látszik, azt lehet késleltetni
natív megoldás van és már multiplatform: loading="lazy"

Fontos, hogy mit késleltetünk
- ha akkor döntjük el, mikor a [[JavaScript|JS]] már fut, késő
- böngésző párhuzamosan tölt és futtat
- csak azt lehet késleltetni, amiről biztosan tudjuk, hogy nem kell TTI-hez
	- Image carousel
	- oldalon később jövő anyagok
	- felhasználói interakcióra töltés
	- ...

## Media formátumok

### Régi, de jól működő formátumok

JPEG
- veszteséges tömörítés, nincs átlátszóság

PNG
- veszteségmentes tömörítés, van átlátszóság

SVG
- vektografikus ábra, vonal, spline, ...

TTF, OTF, WOFF
- font, vektorgrafikus, egyszínű, subpixel rendering
- főleg kicsi képek esetén fontos

### Újabb formátumok

WOFF2
- tudásában azonos, jobban tömörített

WebP
- Erős tömörítés, átlátszóság, veszteséges/mentes
- JPEG-hez képest 30-70%-al kisebb képek azonos minőség mellett.

WebM/VP9 kodek
- videó

AVIF (AV1)
- mégjobb tömörítés, mint a WebP
- HDR támogatás


## Tömörítés

XHR esetén deflate vagy brotli
WebSocket esetén csak deflate
Média avif/webp/webm/stb.
JSS/CSS minified -> és tömörítve
HTTP/2


## HTTP/2 és HTTP/3

HTTP/2
- push resource
- header tömörítés
- multiplexing - egy TCP csatorna több HTTP csomag

HTTP/3
- UDP alapú
- TLS 1.3 beépítve
- Szinte minden böngésző támogatja kliensként
- Minden komolyabb szerver támogatja

## Betűtípusok

Lehet saját betűtípust használni
Formátumok: ttf, otf, woff, woff2
- különbség főleg méretben van
- mind splineból kirakott rajzok, így tetszőleges méretben rajzolhatók

Az ikonokat célszerű így tárolni subpixel rendering miatt
színes nem lehet, de adhatunk neki színt.




