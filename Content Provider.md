---
aliases: 
tags:
  - android
  - uni
  - 5semester
  - mobweb
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-10-15 18:57
---

# Content Provider


>[!tldr]- Motiváció
>Eddigi megoldásaink adatmegosztásra komponensek között korlátozottak voltak, de ez gyakori probléma --> content provider

A *ContentProvider* olyan mechanizmus az [[Android|Androidban]], ami elérési réteget biztosít a strukturált adatokhoz, és elfedi az adat tényleges tárolási módját. Így az adatvédelem biztosítható, és megvalósítható a processzek közti adatmegosztás.

A konkrét adattárolási struktúra vagy módszer tehát az alkalmazásra van bízva, csak egy interfész megvalósítása szükséges (leszármaztatás a *ContentProvider* osztályból és a szükséges metódusok megvalósítása). A legegyszerűbb [[SQLite]] adatbázissal, de nem kötelező ezzel.

---

## ContentProvider elérése

Az adatokat a ContentProvidertől kérdezhetjük le. A komponens, ami képes a  lekérdezések futtatására, és a válasz feldolgozására a *ContentResolver*. Csakis ez tudja lekérdezni a Content Providert. A ContentResolver lehet akár ugyanabban, akár másik processzben is, mit a Content Provider. A kommunikációhoz szükséges IPC-t az [[Android]] elintézi a fejlesztő helyett, teljesen átlátszó. Egy ContentProviderből egyszerre egy példány futhat, ezt éri el az összes Resolver.

### műveletek

Nem csak adatkérés lehetséges, hanem teljes CRUD funkcionalitás:
-> SELECT: `getContentResolver().query(...)`
	visszatérés: cursor az eredményhalmazra
-> INSERT: `getContentResolver().insert(...)`
	visszatérés: a beszúrt adatra mutató URI
-> UPDATE: `getContentResolver().update(...)`
	visszatérés: az update által érintett sorok száma
-> DELETE: `getContentResolver().delete(...)`
	visszatérés: a törölt adatok száma

>[!info]+ CONTENT_URI
>- Azonosítja a Content Providert, és azon belül a táblát
>- pl: UserDictionary.Words.CONTENT_URI = content://user_dictionary/words
>- Felépítése:
>	- content:// - séma, ez mindig jelen van, ebből tudja a rendszer, hogy ez egy Content URI
>	- user_dictionary - "authority", azonosítja a Providert, globálisan egyedinek kell lennie
>	- words - "path", az adattábla (NEM adatbázis tábla) neve amelyre a lekérdezés vonatkozik, egy Provider több táblát is kezelhet.

